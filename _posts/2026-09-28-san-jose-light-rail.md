---
layout: note
title: "Who pays for the train? Designing a study of San José's light rail"
date: 2026-09-28
permalink: /writing/san-jose-light-rail/
excerpt: "A climate-funded light rail should cut emissions and commutes. It may also push up rents, close corner shops and reshape neighbourhoods. A research design for measuring what a project promises and what it does not."
---

Economists have known for a long time that a new rail line changes more than how people travel. When London extended its underground in the late 1990s, house prices near the new stations rose, because being close to a train is worth money ([Gibbons and Machin, 2005](https://doi.org/10.1016/j.jue.2004.10.002)). That is good news if you already own the house. It is less good news if you rent it.

In March 2024 I put together a research design around exactly that tension, for a GCF-funded project: the light rail for the Greater Metropolitan Area of San José, Costa Rica ([FP166](https://www.greenclimate.fund/project/fp166)). The [slides are on GitHub](https://github.com/awongonki/GCF-Finance/blob/main/Presentation_OnKi.pdf). This post is the longer version.

## What the project promises

Costa Rica wants to reach net zero emissions by 2050, and transport is one of its biggest sources of emissions. The light rail is meant to help by moving people out of cars and buses and onto a low carbon system. The funding proposal lists four intended outcomes:

1. more people using a sustainable, low emission urban transport system,
2. stronger capacity to replicate walking, cycling and connectivity projects,
3. lower economic costs of mobility and air pollution, and
4. less dependence on imported fossil fuels.

All of these are worth measuring. None of them is the whole story.

## What it does not promise

A project document describes what a project is designed to do. It rarely describes what else happens. For a rail line running through a capital city, the list of unintended outcomes writes itself:

- **Gentrification and displacement.** If land near the stations becomes more valuable, lower income residents may be priced out of the neighbourhoods the train was supposed to serve.
- **Rising property values and rents** around the stations.
- **Lost small businesses and informal trade**, as land use changes and a food stall becomes a café.
- **Worse congestion during construction**, sometimes for years.
- **Pressure on heritage sites and historic neighbourhoods** along the route.

None of these makes the project a bad idea. But an evaluation that only checks the intended outcomes will miss who carries the costs.

## What I would measure

I grouped the indicators into four families:

- **Transport:** congestion during construction, light rail ridership, and air quality (PM2.5, NOx, SO2).
- **Economic and housing:** property prices and rents near the stations compared with elsewhere, business openings and closures, who is displaced, and income and employment effects across groups.
- **Spatial:** land use change along the corridor, gentrification hotspots, and changes in access to services.
- **What people say:** surveys, interviews and focus groups, including with women, plus the views of local authorities, businesses and residents.

The data mostly exists already: Costa Rica's national household travel survey, traffic counts from the national road agency, ridership from the rail institute, the economic census, house price series, the population census, World Bank living standards surveys, satellite imagery and national geographic data.

## Three methods

**Listening at scale with language models.** Interview transcripts, passenger feedback and social media posts are far too many to read one by one. Large language models can classify each comment as positive, negative or neutral, summarise it, and pull out the themes behind the complaints and the praise. A person still checks a sample, because the model will sometimes be confidently wrong.

**Quantile regression for housing.** An average price effect hides the part of the story that matters. If rail proximity raises prices a little for expensive homes and a lot for cheap ones, the average looks modest while the bottom of the market is transformed. Quantile regression ([Koenker and Bassett, 1978](https://doi.org/10.2307/1913643)) estimates the effect at each point of the price distribution, and the same idea applied to census data on income and education can show whether a neighbourhood is shifting toward richer, more educated residents: the statistical fingerprint of gentrification.

**Change detection from satellites.** High resolution imagery of the corridor, compared over time, shows new construction, demolitions and lost tree cover. Combined with business registrations and the locations of heritage sites, it shows where the landscape is changing and what was there before.

## What I learned from designing it

- The most useful outcomes to measure are often the ones the funding proposal never mentions.
- Averages hide exactly the people a climate project is supposed to protect. Look at the whole distribution.
- Much of the evidence already exists in national statistics and satellite archives. The work is linking it to the project.

This is a research design, not a set of results. The questions stand whoever ends up answering them.

## References

- Gibbons, S., and Machin, S. (2005). Valuing rail access using transport innovations. *Journal of Urban Economics*, 57(1), 148–169.
- Koenker, R., and Bassett, G. (1978). Regression quantiles. *Econometrica*, 46(1), 33–50.
- Green Climate Fund. FP166: Light Rail Transit for the Greater Metropolitan Area (GAM).
