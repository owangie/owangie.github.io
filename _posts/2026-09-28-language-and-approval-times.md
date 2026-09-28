---
layout: note
title: "Does speaking English get your climate project approved faster?"
date: 2026-09-28
permalink: /writing/language-and-approval-times/
excerpt: "A hypothesis from the field, a test on the whole portfolio, one result that disappeared under scrutiny and one that held. From the evaluation of the Green Climate Fund's approach to country ownership."
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

I computed Pearson correlations for every pair, and for the English variable, which is binary, a Welch t-test comparing the average approval time of the two groups.

## What came out

**Portfolio size: nothing.** Neither time zone nor language is related to how many projects a country has or how much money it receives. Every correlation was close to zero.

**Time zone: a result that disappeared.** Across all countries, greater time zone distance looked linked to *shorter* approval times, the opposite of the story we heard. Looking closer, the pattern came from one group: Latin American and Caribbean countries whose projects run through two regional development banks, CAF and the Central American Bank for Economic Integration. Take those countries out and the correlation is no longer significant. Always check what is driving a correlation before you believe it.

**Language: a result that held.** Countries where English is an official language got projects approved faster.

| | Correlation (r) | p-value | t-test |
|---|---|---|---|
| Time zone distance vs. days to approval | –0.05 | 0.60 | |
| English official language vs. days to approval | –0.25 | 0.005 | t = –3.02 |

A correlation of –0.25 is modest. But it is statistically robust, and the t-test tells the same story.

## What it does and does not mean

This is a correlation across countries, not proof that language causes delay. English speaking countries differ in other ways: many share legal and administrative traditions, some have more experience with international funds, and the institutions they work through vary. A proper causal answer would need controls for those differences, or better, variation in language support over time.

What it does show is that the partners' concern is not just an impression. The same pattern shows up in the Fund's own data, which makes it a fair question for policy: if a fund wants every country to own its projects, it should make sure the language of the process is not quietly deciding who moves first.

## Source

Independent Evaluation Unit (2025). *Independent Evaluation of the GCF's Approach to Country Ownership*, Appendix 5: Correlation of time zone and language with GCF country portfolios. [Report page](https://ieu.greenclimate.fund/document/finalreport-coa2025).
