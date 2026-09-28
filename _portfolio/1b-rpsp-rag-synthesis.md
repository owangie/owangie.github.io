---
title: "A RAG pipeline for 1,900 pages of past evaluations"
excerpt: "Retrieval-augmented generation that synthesises 34 prior evaluations and policy documents for the Readiness Programme evaluation, with every claim cited to a document and page."
collection: portfolio
---

The Readiness evaluation needed a baseline: what had earlier evaluations already found, across six evaluation questions? That meant 34 documents and about 1,900 pages of evaluations, management responses, country case studies and policy documents.

I built a retrieval-augmented generation (RAG) pipeline for it. Every passage is indexed in a local vector store with its source, year, page and document type. For each evaluation sub-question, the pipeline retrieves only the most relevant passages and asks a language model to draft that section, citing a specific document and page for every claim. The draft exports to LaTeX for the report and to Word for team review.

The key design choice is one focused retrieval and generation call per sub-question, instead of sending the whole corpus at once. Smaller, relevant evidence sets keep each answer accurate and attributable, and avoid the drop in quality that comes with very long inputs.

*Python, vector store (Chroma), embeddings, LLM API, LaTeX*
