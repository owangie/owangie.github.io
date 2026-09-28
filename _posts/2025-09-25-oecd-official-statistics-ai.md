---
layout: note
title: "What official statisticians are doing to keep AI honest"
date: 2025-09-25
permalink: /writing/oecd-official-statistics-ai/
excerpt: "Notes from an OECD conference on official statistics in Seoul: human review aimed where the model is unsure, RAG versus RIG, and why PDFs are still an unsolved problem."
---

In September 2025 I spent two days (9 and 10 September) at an OECD conference on official statistics in Seoul. Most of the room came from national statistical offices, with speakers from the World Bank, Google and UNICEF. I went because I evaluate projects, and a lot of evaluation now runs on data that AI has touched somewhere along the way. I wanted to see how people who publish numbers for whole countries deal with that.

The short version: they talk about transparency, validation and keeping a human in the loop, which are the same principles evaluators are supposed to live by. They just have them written down in more detail.

## Human review, aimed where the model is unsure

The slide I photographed came from Statistics Korea's research institute. They had piloted AI to code survey answers into statistical classifications, and every prediction came with a probability.

<figure>
  <img src="/images/oecd-2025-hitl-slide.jpg" alt="Slide titled Use-case: Coding, How AI systems are used in practice, showing a bar chart of prediction probabilities by class, a table of low probability predictions boxed in red, and rules for selective manual coding and re-learning" loading="lazy" style="width:100%;border-radius:10px">
  <figcaption style="font-size:13px;color:var(--muted)">Statistics Korea, Statistics Research Institute. The red boxes mark low probability predictions that go to a person. The slide cites UNECE, <em>Machine Learning for Official Statistics</em> (2022).</figcaption>
</figure>

They did not check everything by hand, and they did not trust everything either. A person recoded a case in three situations:

1. the class it belonged to was one the model usually predicted badly,
2. the case itself had a low prediction probability, or
3. the AI disagreed with the older auto-coding system.

The corrected cases then went back in to retrain the model.

This is close to what I had been doing with language models on project documents, where the model can answer "unclear" and those cases go to a reviewer. Their version is easier to audit, though. A probability threshold and a disagreement check are numbers you can report. "The model was unsure" is not.

## Two layers of checks before anything goes out

The World Bank speakers described rule-based checks first (consistency tests, thresholds), then model-based ones such as machine learning anomaly detection, all before a dataset is published. Datasets are described with the [DDI](https://ddialliance.org/) metadata standard, shared through APIs and searchable catalogues, and their use is monitored so the team can tell what is relevant and what needs fixing. Users can send feedback, and it goes back into the next release.

OECD and Eurostat came at it from the rules side. Their AI applications sit under a formal code of practice, the [European Statistics Code of Practice](https://ec.europa.eu/eurostat/web/quality/european-quality-standards/european-statistics-code-of-practice), and they care about the quality of metadata as much as the data itself: how a number was produced and defined is part of the number.

For evaluation this is a useful reference point. Our reports document findings carefully; the datasets behind them deserve the same care.

## RAG, RIG, and a concrete example

Google presented [DataGemma](https://blog.google/technology/ai/google-datagemma-ai-llm/), open models that connect a language model to [Data Commons](https://datacommons.org/), Google's open knowledge graph of public statistics. The UN Statistical Commission already uses Data Commons. The aim is to cut down on made-up numbers, and they showed two ways of doing it.

Take the question: *has the use of renewable energy increased worldwide?*

With **RAG** (retrieval-augmented generation), the system pulls material from a library it has been set up with, then drafts the answer from that. The answer can only be as accurate and as recent as the library.

With **RIG** (retrieval-interleaved generation), the model looks up the figure from a named statistical service while it writes, inserts the current number, and links to the source.

For anyone who puts statistics into reports, RIG is the more interesting one. A number that arrives with a live source link is something a reviewer can check straight away.

The World Bank and Google also presented [Croissant](https://github.com/mlcommons/croissant), a metadata format that makes datasets easier to document and load into machine learning tools.

## The question that did not have an answer yet

A colleague of mine asked the Google speaker whether DataGemma could pull information out of text documents like project funding proposals. The answer was straightforward: it is not used for unstructured text. For structured sources such as SQL databases or JSON, they use in-house language models, but getting information out of PDFs is still an unsolved problem.

That matches my experience. AI for tidy statistical tables is moving quickly. AI for the long, messy documents evaluators actually read will take longer. Until it catches up, the unexciting parts are what make results trustworthy: checks before publishing, a quote behind every extracted claim, and a person looking at the doubtful cases.

## What I took away

- Send cases to people based on how unsure the model is, and report the threshold.
- Document how a dataset was made, not only what it says.
- When a report cites a statistic, link it to a source a reader can check.

## Links

- DataGemma announcement, Google: [blog.google](https://blog.google/technology/ai/google-datagemma-ai-llm/)
- Data Commons: [datacommons.org](https://datacommons.org/)
- Croissant metadata format: [github.com/mlcommons/croissant](https://github.com/mlcommons/croissant)
- DDI metadata standard: [ddialliance.org](https://ddialliance.org/)
- European Statistics Code of Practice: [Eurostat](https://ec.europa.eu/eurostat/web/quality/european-quality-standards/european-statistics-code-of-practice)
- UNECE (2022). *Machine Learning for Official Statistics*.
