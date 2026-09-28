---
layout: note
title: "Screening climate finance proposals with few-shot prompting"
date: 2026-09-28
permalink: /writing/llm-screening-private-sector/
excerpt: "How the GCF private sector evaluation used a language model to read every funding proposal against three criteria, and what it took to make the answers reliable."
---

For the Independent Evaluation of the GCF's Portfolio of and Approach to the Private Sector, I wrote the standard operating procedure (SOP) for an AI-assisted screening of funding proposals, built the pipeline that runs it, and helped build the entity ownership taxonomy it depends on. The method is published in Appendix 4 of the evaluation's [approach paper](https://ieu.greenclimate.fund/sites/default/files/document/260902-priv2026-approach-paper-top.pdf) (pages 30 and 31). This note explains how it works.

## The problem

GCF projects are labelled "Public Sector" or "Private Sector" in Secretariat systems. The label does not show how much the private sector is actually involved in implementation. To answer the evaluation's questions we needed evidence from the text of every approved funding proposal, and reading several hundred long proposals by hand was not realistic.

## Three questions

Each proposal is screened separately against three criteria, each with its own prompt:

* **Market creation.** Does the project deliberately support a local private actor, with its own named component, output or indicator?
* **Risk profile.** Does GCF's financing take a private sector form (loan, guarantee, equity), or does a grant explicitly fund a named mechanism meant to de-risk or mobilise private capital?
* **Capital mobilisation.** Does the proposal name a financing mechanism and an identifiable private counterparty?

For each criterion the model returns "met", "not met" or "unclear", with a direct quotation from the proposal as evidence. Where the evidence is thin it must answer "unclear" rather than guess. A proposal counts as private sector relevant if it meets at least one criterion, because each criterion is a separate route to private sector engagement.

## Few-shot prompting

IBM defines the technique this way: "Few-shot prompting refers to the process of providing an AI model with a few examples of a task to guide its performance." ([IBM](https://www.ibm.com/think/topics/few-shot-prompting))

Each criterion prompt contains real passages copied from proposals, with the correct answer and the reason for it. The most useful examples were the ones the tool first got wrong. An early version marked a proposal as meeting the risk profile criterion because the words "risk" and "debt" appeared close together, when the passage was about protecting the government's own finances. That passage went into the prompt as a corrective example. We repeated the cycle (screen a sample, check every answer against the source text, revise the examples) until the results held up.

## The pipeline

1. **Extract text.** Proposal text comes from PDFs, read page by page with [PyMuPDF](https://pymupdf.readthedocs.io/), or from a JSON archive of approved projects. Text is cached so each proposal is extracted once.
2. **Find the right sections.** Proposal templates have changed over the years, so the code matches section topics (financing structure, institutional arrangements and so on) instead of fixed section numbers.
3. **Ask one question per call.** Three separate calls per proposal, each sending only the sections relevant to that criterion.
4. **Check counterparties.** For capital mobilisation, a named counterparty only counts if it is confirmed private in an entity ownership list. A counterparty not on the list is marked "unclear", never "met".
5. **Review.** The team reviewed every assessment and quotation in the 15 proposal pilot before accepting it.

## The taxonomy

Whether an institution is public or private is not obvious from its name, and the capital mobilisation criterion depends on getting it right. I helped build the entity ownership taxonomy the pipeline checks against.

## Model and settings

The pipeline calls GPT-5.4 mini ([OpenAI](https://openai.com/index/introducing-gpt-5-4-mini-and-nano/)) through GCF's internal Azure AI Foundry deployment, so no proposal text leaves GCF's environment.

* **Temperature 0**, to keep answers as stable as possible across repeated runs.
* **Reasoning effort low.** The model's hidden reasoning uses part of its output budget. A low setting suits a task with explicit rules and worked examples.
* **Output limit of 8,000 tokens per call.** Replies that hit the limit are detected and run again instead of being accepted half finished.

## What I took from it

* The examples in the prompt did more for accuracy than the wording of the instructions.
* Rules that must never be broken belong in code, not in the prompt.
* "Unclear" is a useful answer. It sends a human to the proposals that need one.

## Sources

* IEU (2026). [Independent Evaluation of the GCF's Portfolio of and Approach to the Private Sector: Approach Paper](https://ieu.greenclimate.fund/sites/default/files/document/260902-priv2026-approach-paper-top.pdf), Appendix 4.
* IBM. [What is few-shot prompting?](https://www.ibm.com/think/topics/few-shot-prompting)
* OpenAI. [Introducing GPT-5.4 mini and nano](https://openai.com/index/introducing-gpt-5-4-mini-and-nano/)
* [PyMuPDF documentation](https://pymupdf.readthedocs.io/)
