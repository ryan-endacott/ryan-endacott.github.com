---
layout: post
title:  "The Same Beach Has Killed Hundreds of Whales for Centuries. I Tried to Find Out Why."
date:   2026-03-25 00:00:00
image: /images/whale-strandings-header.jpg
description: "I asked Claude to figure out why whales keep dying at the same beach. It made up the data. Here's what happened when I checked."
excerpt: "**Last year, I asked Claude to help me figure out why pilot whales keep dying at Farewell Spit, New Zealand.** It generated magnetic field measurements, cited NOAA as the source, and confirmed our hypothesis with 100% accuracy. The numbers were completely made up."
---

![A long-finned pilot whale surfacing](/images/whale-strandings-header.jpg)
*Photo: [NOAA](https://unsplash.com/@noaa) via Unsplash*

*Co-authored with [Claude Code](https://claude.ai/code), who did the analysis, wrote the code, ran 15 experiments, and also hallucinated the data that started this whole mess.*

**Last year, I asked Claude to help me figure out why pilot whales keep dying at Farewell Spit, New Zealand.** It generated magnetic field measurements, cited NOAA as the source, and confirmed our hypothesis with 100% accuracy. The numbers were completely made up.

When I came back with Opus 4.6, it caught the fabrication within an hour, ran real experiments with verified data, and built a working early warning prototype. We were feeling pretty good about ourselves.

Then I asked a question the AI never thought to ask, and the whole thing fell apart again.

<!--more-->

## Act 1: A Beautiful Lie

Last May, I asked an AI: *"What's an unsolved scientific mystery that could be tested using only existing public data?"*

It suggested pilot whale mass strandings. Why do hundreds of whales beach themselves at the same specific beaches, over and over, for centuries? Farewell Spit in New Zealand has had 30+ mass strandings. Cape Cod gets them regularly. Meanwhile, beaches just a few miles away get zero.

The AI proposed a hypothesis: **magnetic field gradients create navigational traps.** The field rises as you approach land, creating an invisible "uphill" that confuses whale navigation.

Within 48 hours, we had results:

```python
'Farewell Spit, NZ': {
    'gradient': +20.35,         # nT/km
    'field_ocean': 52585.8,     # nanoTesla
    'field_10km_inland': 52891.1,
}
```

Three sites tested. Three showing the predicted pattern. 100% confirmation. Control sites showed the opposite. It was clean. It was beautiful.

**It was a lie.** The real magnetic field at Farewell Spit is ~56,270 nT, not ~52,586 nT. The gradient doesn't even match the raw values in the same data structure - do the math yourself and you get a different number.

The tricky part: we'd used a Claude browser extension to scrape NOAA's actual online tool. The extension was buggy - it didn't navigate the browser well - and what it saved was somewhere between badly scraped and completely fabricated. But because it appeared to come from NOAA's real tool, we had every reason to trust it. That's what made this dangerous. It wasn't AI making up numbers from nothing. It was a broken tool-use workflow producing plausible garbage from a legitimate source.

## Act 2: The Reckoning

When I came back to the project with Claude Code, things had changed. The models are significantly more capable and less hallucinatory. The first thing it did was audit every number in the codebase. Within an hour:

- **Farewell Spit**: Fabricated field values, ~3,700 nT off from reality
- **Tasmania**: Coordinates **104 km off** - wrong side of the island
- **Matagorda-Padre Island, TX**: Coordinates **99 km off**

When it needed the real magnetic field value, it didn't guess. It installed `ppigrf`, a Python library with the [official IGRF-14 geomagnetic coefficients](https://doi.org/10.5281/zenodo.14012302), wrote three lines of code, and computed the actual answer. Same math every geomagnetic observatory uses.

With verified data across 15 sites, we tested everything:

| Hypothesis | t-statistic | Verdict |
|---|---|---|
| Magnetic field gradient | -0.007 | Dead |
| Inclination gradient | -1.111 | Dead |
| Isoline geometry | -0.659 | Dead |
| Crustal anomalies (3.7km res) | 0.075 | Dead |
| Bathymetry / coastal shape | -1.340 | Dead |

t = -0.007. As null as physics gets. **Eight hypotheses tested across three data resolutions. All dead.**

But instead of stopping there, we asked a different question: **can we predict *when* strandings happen?**

We pulled 20 years of satellite data - sea surface temperature, ocean chlorophyll, ERA5 wind reanalysis - all from free public APIs. Combined it with 20 historical stranding events at Farewell Spit. Built a risk model.

**It worked.** t = 8.09, statistically significant. Every stranding month in our dataset scored above the median risk.

Then we went further. I'd been following Karpathy's [autoresearch](https://github.com/karpathy/autoresearch) - a system that runs ML experiments overnight while you sleep. Same idea, different domain: cache the environmental data, compute 74 features (lags, anomalies, rates of change, multi-month averages), and iterate hypotheses as fast as you can write them. We called it autoscience.

15 experiments in one sitting. Each one testing a different combination - SST anomalies, wind direction, chlorophyll trends, lagged variables, interaction effects. The best model hit t=8.09. But when we stopped optimizing and actually *looked* at the data - compared stranding months to other summer months - only one variable was truly significant: wind speed anomaly (t=2.19). Stranding months are just windier than usual.

And the chlorophyll signal went the wrong direction entirely. LOW chlorophyll predicts strandings, not high. When offshore productivity drops, prey might concentrate closer to shore, drawing whales into the trap.

74 features. 15 experiments. And the honest answer was: "it's summer and it's windy."

I was feeling great. The AI had redeemed itself. Caught its own hallucination, run real analysis, built a working prototype.

Then I asked a question.

## Act 3: The Obvious Question

I was reviewing the results. We had 8 stranding hotspots and 7 control sites. The controls were places with "similar topography but no mass strandings." The Dutch Wadden Sea was our star control - 98% shallow water, the gentlest slope of all 15 sites, and zero strandings. We'd been citing it as proof that coastal geometry alone doesn't predict strandings.

Then it hit me:

*"Do pilot whales even go to the Dutch Wadden Sea?"*

I searched. They don't. **The Wadden Sea is 3 meters deep. Pilot whales feed at 500+ meters and prefer waters over 1,000m deep.** They physically cannot go there. It's like studying tiger attacks and using a parking lot as your control site.

I checked the other controls. Matagorda-Padre Island, Texas - no evidence of pilot whale presence. Banc d'Arguin, Mauritania - no evidence. At least 3 of our 7 control sites are places pilot whales never visit.

The AI - the same AI that caught hallucinated magnetic field values, computed geophysics locally, and downloaded a 1.1 GB crustal anomaly dataset - **never thought to ask whether the animals actually go to the places we were studying.**

Could a reviewer agent have caught this? Probably. "Are these valid control sites for this species?" is exactly the kind of check a review loop would run. The AI wasn't incapable of asking the question - it was in execution mode, and nobody asked it to step back and review the experimental design.

## What This Actually Means

Here's the pattern across the whole project:

- **Round 1 (2025):** AI fabricates data. Human needs to catch bad numbers. *Human fails.*
- **Round 2 (2026):** Better AI catches bad numbers itself. Runs real analysis. Gets real results. *AI succeeds at the technical work.*
- **Round 3 (2026):** Human asks "do whales even go there?" and reveals the whole comparison was flawed.

The lesson isn't "humans are special." A well-designed review step - human or AI - would have caught the control site problem. The lesson is: **the analyst gets tunnel vision, whether it's a person or an AI. You need someone or something reviewing the premise, not just the execution.**

We didn't have that. The AI was busy computing gradients. I was busy being impressed by the gradients. Nobody stepped back to ask "but does this comparison even make sense?"

**AI has automated the easy mistakes and left us with the hard ones.** Catching fabricated numbers? Solved - compute it locally with published coefficients. But questioning whether the entire experimental design makes sense? That requires stepping out of execution mode, and right now, that's still mostly a human reflex. Not because AI can't do it - but because our workflows don't ask it to.

Is this "real science"? A real marine biologist would have caught the control site problem in five seconds. We didn't pre-register hypotheses, we have n=11 events, and the most sophisticated finding is "strandings happen in summer." So... maybe not. But we used real data, real statistical tests, documented every mistake, and caught our own errors in public. I think there's something here, even if it's more "enthusiastic amateur with powerful tools" than "rigorous research." And honestly, I think that's the point - these tools make it possible for anyone to ask real questions of real data. You'll make mistakes. You'll learn. The data doesn't care about your credentials.

*All code, data, and analysis: [github.com/ryan-endacott/madscience](https://github.com/ryan-endacott/madscience). Total cost: ~$200/mo for Claude Max. $0 for data.*
