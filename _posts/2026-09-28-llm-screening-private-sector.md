---
layout: note
title: "Screening climate finance proposals with few-shot prompting"
date: 2026-09-28
permalink: /writing/llm-screening-private-sector/
excerpt: "How the GCF private sector evaluation used a language model to read every funding proposal against three criteria: the architecture, the code, and what it took to make the answers reliable."
---

For the Independent Evaluation of the GCF's Portfolio of and Approach to the Private Sector, I wrote the standard operating procedure (SOP) for an AI-assisted screening of funding proposals, built the pipeline that runs it, and helped build the entity ownership taxonomy it depends on. The method is published in Appendix 4 of the evaluation's [approach paper](https://ieu.greenclimate.fund/sites/default/files/document/260902-priv2026-approach-paper-top.pdf) (pages 30 and 31). This note walks through the design and the code.

## The problem

GCF projects are labelled "Public Sector" or "Private Sector" in Secretariat systems. The label does not show how much the private sector is actually involved in implementation. The evaluation needed that evidence for every approved funding proposal. Proposals are long, and their templates have changed over time. Reading them all by hand, consistently, was not realistic.

So the question for the tool was narrow and testable: **for each proposal, is there written evidence of private sector engagement, and where exactly is it?**

## Three criteria, three separate calls

Each proposal is screened against three criteria. Each criterion has its own prompt and its own model call, so one criterion's answer cannot colour another's.

| Criterion | Question | "Met" requires |
|---|---|---|
| Market creation | Does the project deliberately support a local private actor? | A named component, output or indicator for that support |
| Risk profile | Does GCF's financing take a private sector form? | A loan, guarantee or equity aimed at private investors, or a grant funding a named mechanism to de-risk or mobilise private capital |
| Capital mobilisation | Is private capital actually mobilised? | A named mechanism **and** a named counterparty confirmed private |

Every answer is "met", "not met" or "unclear", and must quote the proposal. A proposal counts as private sector relevant if it meets **at least one** criterion, because each is a separate route to private sector engagement.

## Why few-shot prompting

IBM defines it this way: "Few-shot prompting refers to the process of providing an AI model with a few examples of a task to guide its performance." ([IBM](https://www.ibm.com/think/topics/few-shot-prompting))

Written rules alone were not enough. Words like "risk", "guarantee" and "private sector" appear in almost every proposal, often in senses that have nothing to do with private investment. Worked examples taught the model the difference better than more rules did.

We used **static** few-shot prompting: a fixed, hand-picked set of examples per criterion. Each set is small and specific to one decision, which keeps the prompt short and the behaviour predictable.

## System architecture

<div class="flow">
  <div><b>Proposal</b><span>PDF or JSON record</span></div>
  <div><b>Extract</b><span>PyMuPDF, cached per proposal</span></div>
  <div><b>Target sections</b><span>matched by topic, not number</span></div>
  <div><b>3 model calls</b><span>one per criterion, with examples</span></div>
  <div><b>Rule checks</b><span>in code, after the model</span></div>
  <div><b>Human review</b><span>quote checked against source</span></div>
</div>

* **Extraction.** Text is pulled once per proposal and cached, so reruns do not repeat it.
* **Section targeting.** Each criterion only sees the sections that matter to it (for example financing structure for risk profile), plus a short summary. Less text means lower cost and less room for the model to be distracted.
* **Model.** GPT-5.4 mini ([OpenAI](https://openai.com/index/introducing-gpt-5-4-mini-and-nano/)) through GCF's internal Azure AI Foundry deployment, so no proposal text leaves GCF's environment.
* **Rule checks.** Anything that must never happen is enforced in code, not left to the prompt.

## Implementation

The code below is simplified to show the design. Python packages: `openai`, `azure-identity`, `pymupdf`.

### 1. Extract the text

```python
import fitz  # PyMuPDF

def extract_text(pdf_path):
    with fitz.open(pdf_path) as doc:
        return "\n".join(page.get_text() for page in doc)
```

### 2. Find sections by topic, not by number

The same content can sit under different letters in different template versions. Matching the topic wording survives those changes.

```python
import re

FINANCING_STRUCTURE = re.compile(
    r"(financing\s+structure|financial\s+structure)",
    re.IGNORECASE,
)

def find_section(text, pattern, length=6000):
    match = pattern.search(text)
    if match is None:
        return None          # an honest "not found", never a guess
    return text[match.start(): match.start() + length]
```

### 3. Write the prompt with worked examples

Each criterion prompt has three parts: the decision rule, worked examples, and the required output. The examples that mattered most were the ones the tool first got wrong. The approach paper describes one: an early version marked a proposal as meeting the risk profile criterion because "risk" and "debt" appeared close together, when the passage was about protecting the government's own finances. That case went back into the prompt as a corrective example, in this form (wording illustrative):

```text
EXAMPLE (answer: not_met)
Passage: "...the programme will reduce the country's fiscal risk and
limit new public debt from climate related disasters..."
Why: the risk and debt described belong to the government's own finances.
Nothing here is aimed at private investors.
```

The model must answer in a fixed JSON shape, so every result can be checked and exported the same way:

```json
{
  "status": "met | not_met | unclear",
  "evidence_quote": "exact text copied from the proposal",
  "reasoning": "one or two sentences"
}
```

### 4. Call the model with fixed settings

The settings are the ones published in the approach paper: temperature 0, low reasoning effort, and an 8,000 token output limit.

```python
import os
from azure.identity import DefaultAzureCredential, get_bearer_token_provider
from openai import OpenAI

token = get_bearer_token_provider(DefaultAzureCredential(), "https://ai.azure.com/.default")
client = OpenAI(base_url=os.environ["FOUNDRY_ENDPOINT"], api_key=token)

def ask(system_prompt, user_prompt):
    return client.responses.create(
        model=os.environ["FOUNDRY_DEPLOYMENT"],
        input=[{"role": "system", "content": system_prompt},
               {"role": "user", "content": user_prompt}],
        temperature=0,
        reasoning={"effort": "low"},
        max_output_tokens=8000,
    )
```

Signing in with an Azure identity instead of an API key means there is no secret to store or leak.

### 5. Never accept a cut off answer

GPT-5.4 mini reasons before it answers, and that hidden reasoning counts against the output limit. If the limit is hit, the reply stops mid sentence. The pipeline detects this and runs the call again rather than saving half an answer.

```python
import json

def ask_json(system_prompt, user_prompt, retries=2):
    for _ in range(retries + 1):
        response = ask(system_prompt, user_prompt)
        if response.status == "incomplete":
            continue                      # truncated: try again
        try:
            return json.loads(response.output_text)
        except json.JSONDecodeError:
            continue                      # malformed: try again
    raise RuntimeError("No complete answer after retries")
```

### 6. Enforce hard rules in code

For capital mobilisation, a counterparty only counts if the entity ownership list confirms it is private. A counterparty not on the list is marked "unclear", never "met". The capital mobilisation prompt also returns the counterparty's name, and the rule is applied after the model answers:

```python
def apply_ownership_rule(result, ownership):
    if result["status"] != "met":
        return result
    if ownership.get(result["counterparty"]) != "Private":
        result["status"] = "unclear"
        result["reasoning"] += " Counterparty not confirmed private."
    return result
```

A prompt can ask the model to follow a rule. Only code can guarantee it.

## Quality assurance

The approach paper sets out two safeguards:

1. **Corrective examples** built from real proposal text, including cases the tool got wrong.
2. **Staged testing.** Screen a sample, check every answer by hand against the source, revise the instructions, and repeat until results hold up. The pilot covered 15 proposals chosen to span both Public and Private Sector classifications. The team reviewed every assessment and quotation before accepting it.

## What I would do next

* **Dynamic few-shot prompting.** Instead of a fixed example set, retrieve the examples most similar to the passage being judged, using embeddings and a vector store. Stefan Sipinkoski gives a clear walkthrough with LangChain in "Optimizing AI Agents with Dynamic Few-Shot Prompting" (Medium, 2024). It would let the example library grow without making every prompt longer.
* **Measure agreement formally.** Compare the tool against an independent hand-coded sample and report agreement per criterion, such as Cohen's kappa, so readers can see how far to trust each criterion.

## Lessons

* The examples in the prompt did more for accuracy than the wording of the rules.
* Rules that must never be broken belong in code.
* "Unclear" is a useful answer. It sends a person to the proposals that need one.
* A quote with every answer turns review from rereading a proposal into checking one sentence.

## Sources

* IEU (2026). [Independent Evaluation of the GCF's Portfolio of and Approach to the Private Sector: Approach Paper](https://ieu.greenclimate.fund/sites/default/files/document/260902-priv2026-approach-paper-top.pdf), Appendix 4.
* IBM. [What is few-shot prompting?](https://www.ibm.com/think/topics/few-shot-prompting)
* OpenAI. [Introducing GPT-5.4 mini and nano](https://openai.com/index/introducing-gpt-5-4-mini-and-nano/)
* Sipinkoski, S. (2024). "Optimizing AI Agents with Dynamic Few-Shot Prompting." Medium, 21 November 2024.
* [PyMuPDF documentation](https://pymupdf.readthedocs.io/)
