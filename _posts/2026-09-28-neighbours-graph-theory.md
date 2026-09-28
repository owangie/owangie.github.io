---
layout: note
title: "Are a project's countries neighbours? A short lesson in graph theory"
date: 2026-09-28
permalink: /writing/neighbours-graph-theory/
excerpt: "The maths behind the neighbours chart: countries as a graph, a breadth-first search, a two-step neighbourhood, and one edge case worth knowing about."
---

In the [collection](/#visuals) there is a chart asking whether projects spanning neighbouring countries spend their money more slowly. To draw it, every multi-country project first had to be sorted by one question: *how close are its countries to each other?* This post explains how that sorting works. No maths background needed, but the maths is there if you want it.

## Countries as a graph

A graph, in the mathematical sense, is just dots and lines. Each dot (a *node*) is a country. Each line (an *edge*) joins two countries that are neighbours. Written formally, the graph is **G = (V, E)**, where V is the set of countries and E the set of neighbouring pairs.

Our graph has 243 countries and territories and 588 edges. [Open it here](/assets/graphs/border-graph.html) and click any country to see its neighbours light up.

A project covers a set of countries, call it **S**. The question becomes: how are the dots in S arranged inside G?

## Three tests

**One country.** If S has a single country, the project is *SingleCountry*. Nothing to measure.

**One connected block.** Take only the countries in S and the borders between them. Mathematicians call this the *induced subgraph* of G on S. If you can walk from any country in S to any other, crossing only borders between countries that are themselves in S, the set is *connected*. That project is *StrongRegional*. Kenya, Tanzania and Uganda pass: they all border each other.

**Within two steps.** If S is not one connected block, the code checks something looser. For every country u in S, it collects the countries one step away (its neighbours) and two steps away (its neighbours' neighbours). If at least one other country of S sits in that circle, u passes. If every country passes, the project is *WeakRegional*. Kenya and Rwanda do not share a border, but Uganda sits between them, so they are two steps apart.

Formally, with $$d_G(u,v)$$ the number of borders crossed on the shortest route in G:

$$\text{StrongRegional}(S) \iff G[S] \text{ is connected}$$

$$\text{WeakRegional}(S) \iff \forall\, u \in S \;\; \exists\, v \in S \setminus \{u\} : \; d_G(u,v) \le 2$$

**Everything else** is *MultiRegional*.

<figure style="margin:20px 0"><img src="/images/neighbours-three-cases.png" alt="Three network diagrams: Kenya, Tanzania and Uganda as one connected block; Kenya and Rwanda linked through Uganda; and two separate pairs, Kenya with Uganda and Brazil with Argentina" loading="lazy" style="width:100%"><figcaption style="font-size:13px;color:var(--muted);font-style:italic">The three cases on the real border graph. Navy: the project's countries. Gold: borders between them.</figcaption></figure>

## What the computer actually does

The graph lives in [networkx](https://networkx.org/), the standard Python library for networks. The connected block test calls `nx.is_connected`, which runs a **breadth-first search**: start at any country, visit all its neighbours, then all of theirs, level by level, and count what you reached. If the count equals the size of S, the set is connected. The work grows with the number of countries plus borders, so it is instant even for large projects.

The two-step test is plain set arithmetic. For each country u, build its two-step neighbourhood

$$N_2(u) = N(u) \;\cup \bigcup_{w \in N(u)} N(w)$$

where N(u) is the set of u's neighbours, then intersect it with the rest of the project:

$$N_2(u) \,\cap\, (S \setminus \{u\}) = \varnothing \;\Rightarrow\; u \text{ is isolated}$$

Breadth-first search costs $$O(|V| + |E|)$$: every country and every border is looked at once.

The interactive graph is drawn with [pyvis](https://pyvis.readthedocs.io/), a Python wrapper around the vis-network JavaScript library. Its layout is a physics simulation: edges pull connected countries together like springs, and all nodes push each other apart, until the picture settles. That is why neighbouring regions end up clustered.

## One edge case worth knowing

The two-step rule asks only that each country has *one* partner nearby. So a project covering Kenya, Uganda, Brazil and Argentina passes as WeakRegional: Kenya and Uganda are neighbours, Brazil and Argentina are neighbours, and nobody checks that the two pairs are anywhere near each other. It is rare in real portfolios, but it is the kind of thing to know before you read too much into one category. A stricter version would also require the pairs to link up, for example by checking that S is connected in the graph of countries at most two steps apart.

## Credits

The classification algorithm was written by [Vlad Gerasimov](https://github.com/voismager), software engineer, now at UNICEF. I applied it to disbursement data and built the chart. The border graph was generated with pyvis.
