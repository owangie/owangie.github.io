---
title: "Does readiness support show up in funding proposals?"
excerpt: "An LLM classifier that reads every approved funding proposal and separates real readiness contributions from boilerplate."
collection: portfolio
---

The Fund's Readiness Programme is meant to help countries prepare projects. So a fair question for the Third Performance Review was: how many approved funding proposals say readiness support actually helped build them?

A keyword search does not answer that. "Readiness" appears in template headings, in "investment readiness" and "market readiness", and in country rankings. So the pipeline works in three steps:

1. a keyword pre-filter skips proposals that never mention readiness, with no model call;
2. for the rest, it extracts the text around each mention;
3. a language model classifies those passages against a written rubric (identified needs, developed the concept, financed feasibility studies, fed into a country programme now being financed) and returns structured JSON with its reasoning.

It covered every Board-approved proposal with a public document, about 350 in all. It is a first-pass screen, documented as such, to be checked by people before any number is quoted.

*Python, Azure OpenAI, structured outputs*
