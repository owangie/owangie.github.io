---
featured: true
layout: note
title: "Before you trust a co-financing ratio"
date: 2026-09-28
permalink: /writing/before-you-trust-a-cofinancing-ratio/
excerpt: "Five checks I run before comparing how well climate projects mobilise private capital or disburse on time: whose money it is, what instrument it takes, deal size and timing, mixed averages, and what a regression can and cannot say."
---

"Every dollar of public money brought in three dollars of co-financing." You will find a sentence like that in almost every climate finance report. It is usually true, and it usually tells you less than it seems to.

Over the past year I have spent a lot of time comparing portfolios of climate projects: which business models mobilise more private capital, which ones disburse faster, which ones keep needing to be restructured. The data is public enough and the regressions are standard. What took longer was learning which numbers to distrust. These are the checks I now run every time.

## 1. Whose money is it?

A blended co-financing ratio adds up everything that comes in alongside the fund's own money. A loan from another development bank, a government budget line and a pension fund's equity stake all count the same. For a question about private capital, only the last one matters.

So I split co-financing at the line item: every co-financier on every project, tagged public or private. Rerunning a comparison on private-only money can change which groups look efficient, because some models are very good at attracting other public lenders and much less good at attracting private ones.

The World Bank Group makes the same distinction on its [Scorecard](https://scorecard.worldbank.org/en/outcomes/more-private-investment), which tracks private capital **mobilized** separately from private capital **enabled**. Evaluations should be at least that careful.

## 2. What instrument does it take?

A dollar of grant and a dollar of loan are not the same dollar. The loan comes back; the grant does not. Comparing them at face value flatters whichever portfolio lends more.

Two habits help. First, break co-financing down by instrument (grant, senior loan, subordinated loan, equity, guarantee) and by public or private source, because the same headline ratio can hide very different risk sharing. Second, where the question is about subsidy, value everything in grant equivalent terms, which counts only the concessional part of a loan. For a loan of face value F with principal repayments P and interest payments I in each year t, discounted at rate d:

$$\text{Grant element} = \frac{F - \sum_{t=1}^{T} \dfrac{P_t + I_t}{(1+d)^t}}{F}$$

A grant scores 100%. A loan at the discount rate scores zero. Everything concessional sits in between:

<figure style="margin:20px 0"><img src="/images/grant-element-curve.png" alt="Line chart: grant element of a loan falls from about 62 percent at zero interest to zero at a 5 percent interest rate" loading="lazy" style="width:100%"><figcaption style="font-size:13px;color:var(--muted);font-style:italic">Worked example with a hypothetical loan (30 years, 10 years' grace, 5% discount rate). Not GCF data.</figcaption></figure> I built [a chart on that basis](/#visuals): the private sector share of a climate fund's portfolio, with every loan valued at its grant equivalent.

## 3. Control for size and timing before comparing models

The first comparison is always descriptive: group A has a higher ratio than group B, with a Mann-Whitney test because the ratios are heavily skewed. It is the right place to start and the wrong place to stop.

Business models are not assigned at random. One access modality may take on bigger deals, another may be newer and have had less time to disburse. So I fit regressions that include deal size and approval year alongside the model variables:

- **OLS on the log of the co-financing ratio**, because ratios are skewed and a log makes a doubling mean the same thing at every scale:

  $$\log(\text{CF}_i) = \alpha + \beta\,\text{Model}_i + \gamma \log(\text{Size}_i) + \delta_{\text{year}(i)} + \varepsilon_i$$

  where β is the difference a business model makes once deal size and approval year are held fixed;
- **a logit** for yes or no outcomes, such as whether a project ever needed a formal change request;
- **OLS on the disbursement rate**, and in a separate study on the gap between disbursement and project maturity, with the financial instrument, the number and type of executing entities, and country vulnerability (least developed countries, small island states, region) as explanatory variables;
- **heteroskedasticity robust standard errors (HC3)** throughout, because the variance of a small project's outcome is nothing like a large one's.

The lesson that keeps repeating: some descriptive gaps survive this, some shrink to nothing, and occasionally one flips sign. Reporting only the descriptive table would have been wrong about exactly those.

## 4. Do not trust an average over a mixed group

"International entities perform better than direct access entities" is an average over very different institutions. When I break the same metrics down entity by entity, a handful of large players often drive the whole difference. I flag any cell with fewer than five projects as too thin to read, and I say so in the table rather than in a footnote.

## 5. Say what the regression is not

These are observational data. A coefficient on "multi-country project" tells you how multi-country projects differ once size and timing are held fixed. It does not tell you that making a project multi-country causes slower disbursement. I write findings as observed differences, and I keep the causal language for designs that earn it.

## Why this matters beyond one fund

Every development bank now reports private capital mobilisation, and every board wants to know which approaches deliver. The numbers will only be as good as these unglamorous choices: separating public from private money, respecting the instrument, controlling for the obvious, and not letting an average speak for everyone.

*Tools: Python, pandas, statsmodels (OLS, logit, HC3), SciPy (Mann-Whitney U).*
