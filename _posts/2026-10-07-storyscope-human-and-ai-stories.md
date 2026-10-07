---
featured: true
layout: note
title: "The story is in the structure: what StoryScope found about human and AI fiction"
date: 2026-10-07
permalink: /writing/storyscope-human-and-ai-stories/
excerpt: "A study of 61,608 stories finds that AI fiction can be told apart from human fiction by its structure alone, without looking at a single word choice. What the patterns are, what I take from them for evaluation work, and a writing skill I built from the paper."
---

In March 2026, Hachette pulled the horror novel *Shy Girl* after it was flagged as about 78% AI-generated. It was the first commercially published novel cancelled over AI allegations. That case opens a paper I have not stopped thinking about since: *StoryScope: Investigating idiosyncrasies in AI fiction*, by Jenna Russell, Rishanth Rajendhran, Chau Minh Pham and Mohit Iyyer at the University of Maryland and John Wieting at Google DeepMind ([arXiv 2604.03136](https://arxiv.org/abs/2604.03136), COLM 2026).

Most AI detectors look at words: em dashes, "delve", "tapestry". Those signals are fading. GPT-5.4 already uses fewer em dashes, and a model tuned to imitate human style can bring detection on creative writing from 97% down to 3%, according to work the authors cite. So they asked a harder question. Can you tell a human story from an AI story if you are not allowed to look at the style at all?

## What they did

They took 10,272 published short stories and worked backwards from each to the writing prompt behind it. Five models then wrote a story from the same prompt: Claude Sonnet 4.6, GPT-5.4, Gemini 3 Flash, DeepSeek V3.2 and Kimi K2.5. That gave 61,608 stories of about 5,000 words each.

Each story was turned into a structured template and scored on 304 narrative features: who decides the ending, whether time runs in a straight line, whether the narrator explains the moral. Style features were set aside.

## What they found

**Structure alone gives it away.** With no style information, narrative features separated human from AI stories at 93.2% macro-F1. Adding style barely helped.

**Editing the words does not help.** When Gemini stories were rewritten to strip clichés and other surface artifacts, detection fell only from 95.5% to 93.9%. The kind of story stays the same when the sentences change.

**The models think alike. People do not.** The five models sit in one tight cluster of narrative choices, and human stories as a group sit apart from it, although many individual stories overlap. Given the same prompt, the human story was the most unusual of the six versions 57.8% of the time, where chance would be 16.7%. On average, human stories ranked at the 71st percentile for rarity, AI stories at the 49th.

The differences come down to a handful of habits:

| Habit | AI stories | Human stories |
|---|---|---|
| Narrator comments on the theme or lesson | 77% | 52% |
| No subplots | 79% | 57% |
| Ending driven mainly by the protagonist's own choice | 69% | 46% |
| Protagonist framed as morally mixed | 38% | 59% |
| Emotion shown mainly through the body ("her chest tightened") | 81% | 38% |
| Names specific books or authors | 24% | 47% |

In the paper's words, AI stories "over-explain themes and favor tidy, single-track plots". Human stories leave more unsaid, run more than one thread, let chance decide part of the ending, and move back and forth in time.

Each model also has a fingerprint. Claude keeps events flat and likes an epilogue. GPT turns to gossip and rumour to move the plot. Gemini writes the tidiest endings and the bleakest settings. Kimi has almost no habits of its own and sits in the middle of the pack.

## What I take from it for evaluation work

The study covers fiction, and I want to be careful not to stretch it. But three of its findings change how I read and write evaluation text.

**First, the tell is in the structure, not the vocabulary.** It is tempting to scrub words like "pivotal" from a draft and call it done. This paper suggests that is the least of it. A text that was organised by a model keeps its shape after the words are changed.

**Second, a tidy story should make an evaluator suspicious.** AI fiction resolves its conflict through the hero's own choice. Evaluation reports have the same temptation: every result traced back to the programme's own action, every finding serving one conclusion. Real projects are not like that. Governments change, currencies move, a cyclone arrives. A report in which nothing outside the project ever mattered has told a single-track story, whoever wrote it.

**Third, real texts name real things.** Human authors cite specific books twice as often as the models do. In evaluation, the equivalent is the named document, the page number, the interview in a particular district on a particular date.

## Can we spot a consultant using these tools?

This is the question I keep coming back to. Multilateral banks and UN agencies commission thousands of reports from outside firms. If a firm hands in model-written analysis as its own, the client is paying for judgement it did not get.

My honest answer is that I would not try to catch anyone with a detector. Three reasons:

1. **StoryScope does not cover reports.** It studied 5,000-word short stories. Terms of reference impose a fixed structure on evaluation reports, so narrative habits have less room to show.
2. **Detectors misfire on exactly our writers.** A study in *Patterns* found that GPT detectors flag non-native English writing as AI-generated far more often than native writing ([Liang et al. 2023](https://doi.org/10.1016/j.patter.2023.100779)). Much of the development sector writes in a second or third language.
3. **Using a tool is not the problem.** Passing off its output as fieldwork is.

What does work is checking the things a model cannot invent well, which is the evaluator's job anyway:

- **Trace every citation.** Look up each DOI and report symbol. Invented references are the clearest sign that nobody read the source.
- **Ask for the evidence trail.** Interview lists, dates, site visits, the page behind each quote.
- **Look for the outside world.** A credible finding admits what the project did not control.
- **Look for anything unexpected.** If every finding confirms the theory of change, ask what was found that did not fit.
- **Write disclosure into the terms of reference.** It is easier to ask firms to declare AI use, and how they checked it, than to prove it afterwards.

## A skill built from the paper

I turned the study into a writing skill for Claude and other language models: [**storyscope-writing-skill**](https://github.com/awongonki/storyscope-writing-skill). It takes the 30 core features and the model fingerprints, with the paper's numbers, and turns them into two workflows. One plans a story from a "structure card" before any sentence is written. The other audits a finished draft against the six default patterns and proposes structural changes, not word swaps. A script in the repository recomputes the figures from the authors' released data.

It is unofficial. The authors did not write or endorse it. It is a craft aid for fiction and narrative essays. It will not make text undetectable, and it does not change anyone's duty to disclose AI help.

## One last detail

Near the end of the paper, in its ethics statement, the authors disclose that they used Claude Code and Codex "to aid with and polish writing". A study about what machines do to stories was written with machines in the room.

I did the same with this post. I drafted it with an AI assistant and revised it with the skill above. The questions, the reading of the paper and the views are mine. Whether the structure is, I will leave to you.

---

*Disclosure: This post was drafted with an AI assistant and revised using the storyscope-narrative-craft skill. It is a personal view and does not represent the organizations I work for or have worked for. The skill is an independent interpretation of published research and is not endorsed by the StoryScope authors.*

*Source: Russell, J., Rajendhran, R., Pham, C. M., Iyyer, M. and Wieting, J. (2026). StoryScope: Investigating idiosyncrasies in AI fiction. COLM 2026. [arXiv:2604.03136](https://arxiv.org/abs/2604.03136). Code and data: [github.com/jenna-russell/storyscope](https://github.com/jenna-russell/storyscope).*
