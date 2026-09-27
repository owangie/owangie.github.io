---
title: "AI screening of a climate finance portfolio"
excerpt: "An LLM pipeline that reads every GCF funding proposal and classifies it against evaluation criteria, with an audit trail a human reviewer can check."
collection: portfolio
---

**Problem.** An evaluation needed to identify which projects in a portfolio of several hundred funding proposals met a set of private sector criteria. Manual review would take months and be hard to reproduce.

**What I built**
* A Python pipeline on Azure AI Foundry that extracts the relevant sections of each proposal and asks a GPT model to apply a written codebook
* Structured outputs with the quoted evidence behind each classification, so reviewers can verify rather than trust
* A validation step against a manually coded sample, and fixes for failure modes found along the way (truncated documents, missing entity information)
* Scaled from a pilot to the full public sector portfolio

**Why it matters for outcomes work.** The same design (codebook, extraction, evidence citation, human validation) applies to reading results frameworks in PADs, ISRs and ICRs at scale.

*Tools: Python, Azure AI Foundry, GPT models, pandas, Git*
