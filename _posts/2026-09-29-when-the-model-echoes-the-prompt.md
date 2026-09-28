---
layout: note
title: "What if the model just hands your question back?"
date: 2026-09-29
permalink: /writing/when-the-model-echoes-the-prompt/
excerpt: "A new study finds that small language models sometimes answer a prompt by copying it, more often at low temperature and after quantisation. Why that matters for any pipeline that asks a model to quote its evidence, and a short check for it."
---

I did not know a language model could do this. You send it a prompt, and instead of answering, it starts its reply by copying your prompt back, word for word. Sometimes the whole thing. Sometimes the first half, before it wanders off.

A new paper, *Mirror, Mirror in the Wall*, submitted to EMNLP with its [data and code on GitHub](https://github.com/CredibleAI/Echo), measures how often this happens. It tests small open models, from 1.7 to 13 billion parameters, on three sets of prompts: two built to trigger echo, and one reference set that triggers none.

## What counts as an echo

The paper uses two definitions, both about the start of the answer:

- **Full echo**: the answer begins with the complete prompt, copied exactly.
- **Partial echo**: the answer begins with an exact copy of at least 40% of the prompt's tokens, then diverges.

In symbols, if the prompt is the token sequence p<sub>1</sub>, …, p<sub>n</sub> and the answer is a<sub>1</sub>, a<sub>2</sub>, …, let L be the length of their common prefix:

$$L = \max \{\, k : a_i = p_i \text{ for all } i \le k \,\}$$

Then the answer is a full echo when L = n, and a partial echo when 0.4n ≤ L < n.

## What they found

Three patterns stand out in the paper's first experiment (Table 1):

- **Lower temperature, more echo.** Temperature controls how much randomness goes into picking each next word. At 0 the model always takes its single most likely word. For SmolLM2 1.7B on the second prompt set, the share of answers that echo rose from about 2% at temperature 1.0 to about 12.5% at temperature 0.
- **Compression makes it worse.** The same model compressed to 4 bits, a common way to run models on a laptop, echoed in about 15% of answers at temperature 0.
- **It is not every model.** Qwen3.5 4B almost never echoed, under a tenth of a percent on the first prompt set. Model choice matters as much as settings.

The later experiments look for causes: whether echoing prompts resemble the models' training data, and what happens when you switch off the attention heads that are best at copying sequences. I will leave those to the paper.

## Why an evaluator should care

Most of my pipelines ask a model to read a document and **quote the passage** that supports its answer. Pipelines like that are often run at low temperature, because you want the same answer every time. That is exactly the setting where this paper finds more echo.

An echo in that pipeline would not look like an error. The "quote" would be real text. It would just come from my prompt, the rubric or the instructions, rather than from the project document. A reviewer skimming the output could easily take it as evidence.

The paper tests small open models, not the large hosted ones I mostly use, so I would not assume the same rates. But the check is cheap, so I would rather run it than assume.

## A short check

Before trusting a quote, confirm two things: it appears in the source document, and it does not start as a copy of the prompt.

```python
def prefix_echo(prompt_tokens, answer_tokens, partial=0.4):
    """Classify an answer as 'full', 'partial' or no echo of the prompt."""
    L = 0
    for p, a in zip(prompt_tokens, answer_tokens):
        if p != a:
            break
        L += 1
    n = len(prompt_tokens)
    if n and L == n:
        return "full"
    return "partial" if n and L >= partial * n else None

def quote_is_grounded(quote, document):
    return " ".join(quote.split()) in " ".join(document.split())
```

Flag any row where `prefix_echo` returns something, or `quote_is_grounded` returns False, and send it to a person. In a screen of a few hundred proposals that is a handful of rows, and it closes a gap that would otherwise stay invisible.

## Source

*Mirror, Mirror in the Wall* (2026), submitted to EMNLP. Data and code: [github.com/CredibleAI/Echo](https://github.com/CredibleAI/Echo). Numbers above are read from the paper's Table 1, Experiment 1.
