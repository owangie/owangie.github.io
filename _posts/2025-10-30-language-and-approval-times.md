---
layout: note
title: "Does speaking English get your climate project approved faster?"
date: 2025-10-30
permalink: /writing/language-and-approval-times/
excerpt: "A hypothesis from the field, a test on the whole portfolio, one result that disappeared under scrutiny and one that held, and how both the Secretariat and civil society cited it in the Board debate on multilingualism."
---

The hypothesis came from the field, not from a spreadsheet. In interviews for the [evaluation of the Green Climate Fund's approach to country ownership](https://ieu.greenclimate.fund/document/finalreport-coa2025), partners from the World Bank, UNDP, FAO and others kept telling us the same thing. The Fund works almost entirely in English from its headquarters in Korea, with no regional or country offices. Many of the people preparing projects on the ground do not work in English, and some are many time zones away.

So: is language a barrier? Does it slow projects down?

My first idea was to measure it directly, by looking at how long emails took to get a reply. That idea lasted about one meeting. Mining colleagues' inboxes is, understandably, not something HR lets an evaluation do. So I looked for a signal in data we could use.

## The test

From the Secretariat's project data I took every approved project and counted the days from first submission to Board approval. Then, for each country:

- the **average approval time** across its projects,
- whether **English is an official language** (1 or 0),
- its **time zone distance** from Korea (UTC offset relative to Korea Standard Time, UTC+9),
- its **number of projects** and **total financing**.

I computed Pearson correlations for every pair:

$$r = \frac{\sum_i (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_i (x_i - \bar{x})^2 \; \sum_i (y_i - \bar{y})^2}}$$

and for the English variable, which is binary, a Welch t-test comparing the average approval time of the two groups, which does not assume they have the same variance:

$$t = \frac{\bar{y}_{\text{English}} - \bar{y}_{\text{other}}}{\sqrt{s^2_{\text{English}} / n_{\text{English}} + s^2_{\text{other}} / n_{\text{other}}}}$$

## What came out

**Portfolio size: nothing.** Neither time zone nor language is related to how many projects a country has or how much money it receives. Every correlation was close to zero.

**Time zone: a result that disappeared.** Across all countries, greater time zone distance looked linked to *shorter* approval times, the opposite of the story we heard. Looking closer, the pattern came from one group: Latin American and Caribbean countries whose projects run through two regional development banks, CAF and the Central American Bank for Economic Integration. Take those countries out and the correlation is no longer significant. Always check what is driving a correlation before you believe it.

**Language: a result that held.** Countries where English is an official language got projects approved faster.

| | Correlation (r) | p-value | t-test |
|---|---|---|---|
| Time zone distance vs. days to approval | –0.05 | 0.60 | |
| English official language vs. days to approval | –0.25 | 0.005 | t = –3.02 |

<figure style="margin:20px 0"><img src="/images/language-correlations.png" alt="Lollipop chart of six correlations; only English official language versus days to approval, r = -0.25, is significant" loading="lazy" style="width:100%"><figcaption style="font-size:13px;color:var(--muted);font-style:italic">All six published correlations. Only one clears the bar.</figcaption></figure>

A correlation of –0.25 is modest. But it is statistically robust, and the t-test tells the same story.

## What it does and does not mean

This is a correlation across countries, not proof that language causes delay. English speaking countries differ in other ways: many share legal and administrative traditions, some have more experience with international funds, and the institutions they work through vary. A proper causal answer would need controls for those differences, or better, variation in language support over time.

What it does show is that the partners' concern is not just an impression. The same pattern shows up in the Fund's own data, which makes it a fair question for policy: if a fund wants every country to own its projects, it should make sure the language of the process is not quietly deciding who moves first.

## Where the finding went

The analysis did not stay in an appendix. When the Secretariat brought its approach to multilingualism to the Board in October 2025 ([GCF/B.43/Inf.12](https://www.greenclimate.fund/board-document/gcf-b43-inf12)), it cited both results:

> *"the recent IEU evaluation on the GCF's Approach to Country Ownership (B.43/04) found a modest but statistically robust correlation between English as an official language and shorter approval times"*

and

> *"close to zero systematic association between English as an official language and the number of projects and size of a country's GCF portfolio."*

The paper weighed the two together. It concluded there was not yet broad evidence that accepting proposals in other languages would by itself widen access, and focused its approach on language skills: hiring and training so that regional teams and headquarters can work with countries in their own languages.

Civil society read the same evidence very differently. At the same Board meeting, the GCF Observer Network of Civil Society, Indigenous Peoples, and Local Communities called the paper *"under-developed documentation"* rather than a real strategy ([intervention on multilingualism](https://www.gcfwatch.org/wp-content/uploads/2025/10/GCFWatch_B.43_Multilingualism.pdf)). It pointed to the same country ownership evaluation as evidence that *"the wide participation from stakeholders, key to country ownership, is limited in those countries in which English is not a widely spoken language,"* and asked for funding proposals to be available in the languages of the countries where projects run, including Indigenous languages where Indigenous Peoples are affected.

One evaluation, two readings: the Secretariat saw a modest effect on speed and no effect on access; civil society saw a barrier to participation that the numbers only begin to capture. Both are fair readings of different parts of the evidence. That is what a small piece of analysis can do. It did not settle the question, and it was never going to. It gave a policy discussion a number to argue with instead of an impression, and it showed which part of the problem, speed rather than access, the evidence actually points to.

## Sources

Independent Evaluation Unit (2025). *Independent Evaluation of the GCF's Approach to Country Ownership*, Appendix 5: Correlation of time zone and language with GCF country portfolios. [Report page](https://ieu.greenclimate.fund/document/finalreport-coa2025).

Green Climate Fund (2025). *Multilingualism*. GCF/B.43/Inf.12, Meeting of the Board, 27–30 October 2025. [Board document](https://www.greenclimate.fund/board-document/gcf-b43-inf12).

GCF Observer Network of Civil Society, Indigenous Peoples, and Local Communities (2025). *Intervention on Multilingualism*, 43rd Meeting of the Board. [GCFWatch](https://www.gcfwatch.org/wp-content/uploads/2025/10/GCFWatch_B.43_Multilingualism.pdf).
