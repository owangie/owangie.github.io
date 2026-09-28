---
layout: note
title: "Pipeline, portfolio, Indigenous: what language models get wrong about development work"
date: 2026-09-28
permalink: /writing/llms-and-development-jargon/
excerpt: "What I have learned pointing language models at development documents. The cost fell from dollars to cents. The hard part stayed the same: finding the right passage, and teaching the model what our words mean."
---

Since 2024 I have been pointing language models at development documents: project proposals, progress reports, evaluations. Along the way I have tried most of the big names side by side, GPT, Claude, Grok, Perplexity and open models on Hugging Face, on the same tasks.

Two things surprised me. The first is how cheap this has become. The second is that the hard problems have nothing to do with which model you pick.

## From dollars to cents

Last year, working with GPT-4o, a run cost me somewhere between one and twenty dollars depending on how much text I sent. This year, turning more than 200 PDF proposals into structured JSON with GPT-5.4 mini cost about fifty cents.

At that price, cost stops being the constraint. What limits the quality of the answer is everything around the model.

## Hard problem one: the right passage from the right place

A long funding proposal is mostly noise for any single question. Budget tables, annexes, boilerplate about safeguards, a letter from a ministry. If you hand all of it to the model, it will find something that looks like an answer, often in the wrong place.

The biggest gains I have had came from deciding *where* the model should look before deciding *what* to ask it. For a question about how a project is financed, send the financing section. For a question about who implements it, send the institutional arrangements. In my experience, less text chosen well beats more text.

## Hard problem two: our words mean different things

Development work has its own dialect, and models trained on the whole internet do not speak it.

- **Pipeline.** To an engineer it is oil or gas. To a data scientist it is a chain of processing steps. To a climate fund it is the set of projects under preparation that have not yet been approved.
- **Portfolio.** In finance it is a set of investments you hold. In our world it is the set of approved projects, and "portfolio performance" is about results and disbursement, not returns.
- **Accredited entity, readiness, co-financing, paradigm shift.** Each has a precise meaning inside a climate fund that a general model will happily guess at.

So before any prompt, I write a taxonomy: a short list of the terms that matter for the question, each with a plain definition and an example of how it appears in real documents. For one study we also built an entity ownership list, because whether an institution is public or private is rarely obvious from its name. The model gets those definitions with the prompt.

## A keyword is not a finding

Work on Indigenous Peoples taught me the sharpest version of this. The obvious approach is to search for "Indigenous" and count the hits. But many of the sentences that contain the word say the opposite of what a count would suggest. Sentences like these (illustrative, not quoted from any one document):

1. *"No Indigenous Peoples have been identified in the project area."*
2. *"An Indigenous Peoples Plan will be prepared if required."*
3. *"Indigenous women's cooperatives will co-manage the mangrove restoration component and share its revenues."*

All three contain the keyword. Only the third describes Indigenous Peoples actually taking part in the project. The first is a screening statement that rules them out. The second is a promise to do something later, only if needed.

A keyword search counts all three. A model given clear definitions, and a few examples like these with the right answer next to them, can tell them apart, and it can quote the sentence that decided it so a person can check.

## What I would tell anyone starting

- Spend your first hour on *where* the answer lives, not on the prompt.
- Write down what your key terms mean in your organisation, and give the model that list.
- Collect the sentences that fool a keyword search. They make the best examples.
- Compare a few models on your own documents before settling on one.
