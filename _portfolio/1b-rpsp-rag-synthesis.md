---
featured: true
title: "A RAG pipeline for 1,900 pages of past evaluations"
excerpt: "Retrieval-augmented generation that synthesises 34 prior evaluations and policy documents for the Readiness Programme evaluation, with every claim cited to a document and page."
collection: portfolio
---

The Readiness evaluation needed a baseline: what had earlier evaluations already found, across six evaluation questions? That meant 34 documents and about 1,900 pages of evaluations, management responses, country case studies and policy documents.

I built a retrieval-augmented generation (RAG) pipeline for it. Every passage is indexed in a local vector store with its source, year, page and document type. For each evaluation sub-question, the pipeline retrieves only the most relevant passages and asks a language model to draft that section, citing a specific document and page for every claim. The draft exports to LaTeX for the report and to Word for team review.

The key design choice is one focused retrieval and generation call per sub-question, instead of sending the whole corpus at once. Smaller, relevant evidence sets keep each answer accurate and attributable, and avoid the drop in quality that comes with very long inputs.

The next improvement I would make is [contextual retrieval](https://www.anthropic.com/engineering/contextual-retrieval) (Anthropic, 2024). A chunk such as "the programme met its target" is useless on its own: which programme, which evaluation, which year? Contextual retrieval asks a model to write a short note placing each chunk in its document, prepends it before indexing, and pairs embeddings with BM25 keyword search. In Anthropic's tests this cut failed retrievals by 49%, and by 67% with reranking. Evaluation corpora are full of exactly these orphaned sentences.

<figure style="margin:12px 0"><img src="/images/contextual-retrieval-evaluation.png" alt="Diagram: evaluation corpus is split into chunks, a language model writes context for each chunk, context plus chunk is indexed with embeddings and BM25, and a sub-question searches both before reranking and drafting with citations" loading="lazy" style="width:100%;background:#fff"><figcaption style="font-size:12px;color:var(--muted);font-style:italic">How contextual retrieval would work on evaluation documents. My own diagram, adapted from Anthropic (2024).</figcaption></figure>

*Python, vector store (Chroma), embeddings, LLM API, LaTeX*
