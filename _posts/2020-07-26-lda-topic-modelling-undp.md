---
layout: note
title: "What do UNDP's innovation projects talk about? A step by step guide to LDA topic modelling"
date: 2020-07-26
permalink: /writing/lda-topic-modelling-undp/
excerpt: "A guide I wrote at UNDP in 2020: translating project descriptions, cleaning text, building a document-term matrix, choosing the number of topics by coherence, and reading the topics with a dendrogram. In R, with the code."
---

In 2020, UNDP country offices described their innovation work in free text: accelerator labs, youth bootcamps, gender-based violence platforms, new ways of financing the SDGs. Nobody had time to read all of it and sort it by hand. So I wrote this guide for colleagues on how to let a topic model do the first pass.

The method is **Latent Dirichlet Allocation (LDA)**. It assumes each document is a mix of topics, and each topic is a mix of words. Given only the documents, it works backwards to find the topics.

## The idea in one formula

LDA imagines each document being written like this: pick a mix of topics θ for the document; then, for every word, pick a topic z from that mix, and pick a word w from that topic's word distribution φ. The probability of seeing word w in document d is

$$P(w \mid d) = \sum_{k=1}^{K} \underbrace{P(w \mid z = k)}_{\varphi_{k,w}} \; \underbrace{P(z = k \mid d)}_{\theta_{d,k}}$$

The model's job is to find the φ (what each topic is about) and the θ (what each document is about) that best explain the text we actually have. **φ** is what we read to name a topic; it is the "phi" value in the code below.

## Step 1. Get everything into one language

The descriptions came in English, Spanish, French and more. In a spreadsheet, `=DETECTLANGUAGE(text)` gives the language code, and `=GOOGLETRANSLATE(text, source, "en")` translates each cell into English. Then Text to Columns with a line break (Ctrl + J) as the delimiter splits each description into paragraphs, one per row, each with a unique ID. A topic model works better on paragraphs than on long documents that mix several themes.

## Step 2. Clean out the noise

Some words appear everywhere and say nothing about the topic: *UNDP*, *project*, *support*, *develop*, country names, numbers, punctuation and common stop words. They go first.

```r
library(textmineR); library(tidytext); library(dplyr); library(tidyr); library(readxl)

data <- read_excel("innovation_descriptions.xlsx") |>
  select(ID, Cleaned_Translation) |> na.omit()

# words that are frequent but not informative for this corpus
noise <- c("support", "people", "develop", "developed", "development", "establish",
           "established", "provide", "provided", "providing", "approach", "approaches",
           "increase", "conduct", "process", "test", "tested", "sdg")
pattern <- paste0("\\b(", paste(noise, collapse = "|"), ")\\b")   # whole words only
data$Cleaned_Translation <- gsub(pattern, "", data$Cleaned_Translation, ignore.case = TRUE)

tokens <- data |>
  unnest_tokens(word, Cleaned_Translation) |>
  mutate(word = gsub("[[:digit:]]+|[[:punct:]]+", "", word)) |>
  filter(nchar(word) > 2) |>
  anti_join(stop_words, by = "word") |>
  group_by(ID) |> summarise(Cleaned_Translation = paste(word, collapse = " "))
```

## Step 3. Build the document-term matrix

A document-term matrix has one row per paragraph and one column per term, counting how often each term appears. Allowing two-word terms keeps phrases like *accelerator lab* and *gender based* together. Terms that appear only once, or in more than half of all paragraphs, are dropped: too rare to form a topic, or too common to separate one.

```r
dtm <- CreateDtm(tokens$Cleaned_Translation, doc_names = tokens$ID,
                 ngram_window = c(1, 2),
                 stem_lemma_function = function(x) SnowballC::wordStem(x, "porter"))
tf <- TermDocFreq(dtm)
vocabulary <- tf$term[tf$term_freq > 1 & tf$doc_freq < nrow(dtm) / 2]
dtm <- dtm[, vocabulary]
```

## Step 4. How many topics?

LDA needs the number of topics K up front, and that is the hardest choice. Rather than guess, fit a model for every K from 1 to 20 and score each one by **probabilistic coherence**: for the top words a and b of a topic, how much more likely is b in a paragraph that contains a than in a paragraph picked at random?

$$\text{coherence} = \operatorname{mean}_{a,b \,\in\, \text{top words}} \big[ P(b \mid a) - P(b) \big]$$

A high score means the topic's words really do travel together.

```r
models <- lapply(1:20, function(k) {
  m <- FitLdaModel(dtm = dtm, k = k, iterations = 500)
  m$coherence <- CalcProbCoherence(phi = m$phi, dtm = dtm, M = 5)
  m
})
coherence <- data.frame(k = 1:20, coherence = sapply(models, \(m) mean(m$coherence)))
```

<figure style="margin:20px 0"><img src="/images/lda-coherence.png" alt="Line chart of mean coherence by number of topics from 1 to 20; the highest point is at 16 topics" loading="lazy" style="width:100%"><figcaption style="font-size:13px;color:var(--muted);font-style:italic">Mean coherence by number of topics. It climbs quickly, then levels off; 16 topics scored highest.</figcaption></figure>

## Step 5. Read the topics

For each topic, list the ten words with the highest φ, the words most likely to be drawn from that topic.

```r
model <- models[[16]]
top_terms <- GetTopTerms(phi = model$phi, M = 10)
```

<figure style="margin:20px 0"><img src="/images/lda-top-terms.png" alt="Table of the top ten terms for each of 16 topics, for example accelerator lab, youth entrepreneurship, gender based violence, health and HIV, governance bodies" loading="lazy" style="width:100%"><figcaption style="font-size:13px;color:var(--muted);font-style:italic">Top ten terms for each of the 16 topics.</figcaption></figure>

Some topics name themselves: accelerator labs (t_7), youth and entrepreneurship (t_5), gender-based violence (t_10), health and HIV (t_13), governance bodies (t_14), climate and rural communities (t_15). That is the point where a person takes over from the model and gives each topic a name.

## Step 6. Which topics are really the same?

Some topics share words. To see which ones are close, measure the distance between their word distributions with the **Hellinger distance**,

$$H(\varphi_i, \varphi_j) = \frac{1}{\sqrt{2}} \sqrt{\sum_w \left( \sqrt{\varphi_{i,w}} - \sqrt{\varphi_{j,w}} \right)^2}$$

which is 0 for identical topics and 1 for topics with no words in common, then cluster them.

```r
model$dist <- CalcHellingerDist(model$phi)
model$hclust <- hclust(as.dist(model$dist), "ward.D")
plot(model$hclust)
```

<figure style="margin:20px 0"><img src="/images/lda-dendrogram.png" alt="Cluster dendrogram of the 16 topics using Hellinger distance and Ward linkage" loading="lazy" style="width:100%"><figcaption style="font-size:13px;color:var(--muted);font-style:italic">Topics that join low on the tree are closest.</figcaption></figure>

The tree pairs t_2 with t_3, t_11 with t_12, t_5 with t_9, and t_4 with t_10. But the word lists show those pairs are still about different things, so all 16 stayed. The dendrogram is a prompt to look again, not an instruction to merge.

## What I would do differently now

This was 2020. Today I would still start with a topic model to see the shape of a corpus quickly and cheaply, then use a language model with written definitions to classify each paragraph against a fixed list of themes, with a quote as evidence ([how I do that now](/writing/llm-screening-private-sector/)). Topic models find what is there; classification tells you how much of what you were looking for.

## References

- Blei, D. M., Ng, A. Y. and Jordan, M. I. (2003). Latent Dirichlet Allocation. *Journal of Machine Learning Research*, 3, 993–1022.
- [textmineR package](https://cran.r-project.org/package=textmineR), which provides CreateDtm, FitLdaModel, CalcProbCoherence and CalcHellingerDist.
- [Text mining with R: topic models](https://books.psychstat.org/textmining/topic-models.html).
- [GOOGLETRANSLATE in Google Sheets](https://support.google.com/docs/answer/3093331?hl=en).
