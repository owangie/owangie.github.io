---
title: "Mapping 15,000 UNDP evaluations to the SDGs"
excerpt: "Topic modelling on mid-term and final evaluation reports, matched to Sustainable Development Goal categories."
collection: portfolio
---

UNDP had more than 15,000 mid-term and final evaluation reports, and no quick way to see which development goals they spoke to. I matched the reports to Sustainable Development Goal categories and used bag of words topic modelling (Latent Dirichlet Allocation) to find the themes running through them, then visualised the topics so colleagues could explore them. The results fed corporate reporting, portfolio analysis and performance monitoring.

The same approach, applied to UNDP's innovation projects, is written up step by step in [a guide I wrote in 2020](/writing/lda-topic-modelling-undp/), with the [R code on GitHub](https://github.com/awongonki/undp_LDA_2020).

*Python: scikit-learn (CountVectorizer, LatentDirichletAllocation), gensim, nltk, spaCy, pyLDAvis. R: tm, topicmodels, tidytext, quanteda, LDAvis.*
