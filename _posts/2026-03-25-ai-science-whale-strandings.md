---
layout: post
title:  "I Used AI to Do Real Science. It Hallucinated the Data."
date:   2026-03-25 00:00:00
image: /images/whale-strandings-header.png
description: "AI fabricated scientific data with decimal precision and cited sources. Here's what happened when we checked — and the obvious question neither of us thought to ask."
excerpt: "**AI fabricated scientific data — decimal-precise measurements with realistic units and cited sources.** When we came back and actually verified, better AI caught the hallucination, ran real experiments, and built a working prototype. We were feeling pretty good. Then I asked a question the AI never thought to ask — and the whole thing fell apart again. This is a story about AI getting smarter, and why that makes humans *more* important, not less."
---

![A pod of pilot whales approaching a hook-shaped coastline](/images/whale-strandings-header.png)

*Co-authored with Claude (Opus 4.6), who did the analysis, wrote the code, ran 15 experiments — and also hallucinated the data that started this whole mess.*

**AI fabricated scientific data — decimal-precise measurements with realistic units and cited sources.** When we came back and actually verified, better AI caught the hallucination, ran real experiments, and built a working prototype. We were feeling pretty good about ourselves.

Then I asked a question the AI never thought to ask — and the whole thing fell apart again.

This is a story about AI getting smarter, and why that makes humans *more* important, not less.

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

**It was a lie.** The real magnetic field at Farewell Spit is ~56,270 nT, not ~52,586 nT. The numbers were invented. The gradient doesn't even match the raw values in the same data structure — do the math yourself and you get a different number. The AI cited "NOAA WMM-2010" as the source. NOAA had nothing to do with it.

## Act 2: The Reckoning

When I came back to the project with Claude Code, the tooling had changed significantly. Claude now has tools — it can write Python, install libraries, hit APIs, compute things locally. Models are genuinely less hallucinatory. It's a different game.

The first thing it did was audit every number in the codebase. Within an hour:

- **Farewell Spit**: Fabricated field values, ~3,700 nT off from reality
- **Tasmania**: Coordinates **104 km off** — wrong side of the island
- **Matagorda-Padre Island, TX**: Coordinates **99 km off**

When it needed the real magnetic field value, it didn't guess. It installed `ppigrf` — a Python library with the [official IGRF-14 geomagnetic coefficients](https://doi.org/10.5281/zenodo.14012302) — wrote three lines of code, and computed the actual answer. Same math every geomagnetic observatory uses.

Today's models are also genuinely less hallucinatory — more likely to say "I don't know, let me check" than to confidently invent an answer. Both the models and the workflows got better.

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

We pulled 20 years of satellite data — sea surface temperature, ocean chlorophyll, ERA5 wind reanalysis — all from free public APIs. Combined it with 20 historical stranding events at Farewell Spit. Built a risk model.

**It worked.** t = 8.09, statistically significant. Every stranding month in our dataset scored above the median risk. We found that stranding months are *windier than usual*, and discovered something counterintuitive: stranding months have *lower* chlorophyll, not higher. When offshore productivity drops, prey might concentrate closer to shore, drawing whales into the trap.

I was feeling great. The AI had redeemed itself — caught its own hallucination, run real analysis, built a working prototype.

Then I asked a question.

## Act 3: The Obvious Question

I was reviewing the results. We had 8 stranding hotspots and 7 control sites. The controls were places with "similar topography but no mass strandings." The Dutch Wadden Sea was our star control — 98% shallow water, the gentlest slope of all 15 sites, and zero strandings. We'd been citing it as proof that coastal geometry alone doesn't predict strandings.

Then it hit me:

*"Do pilot whales even go to the Dutch Wadden Sea?"*

I searched. They don't. **The Wadden Sea is 3 meters deep. Pilot whales feed at 500+ meters and prefer waters over 1,000m deep.** They physically cannot go there. It's like studying tiger attacks and using a parking lot as your control site.

I checked the other controls. Matagorda-Padre Island, Texas — no evidence of pilot whale presence. Banc d'Arguin, Mauritania — no evidence. At least 3 of our 7 control sites are places pilot whales never visit.

The AI — the same AI that caught hallucinated magnetic field values, computed geophysics locally, and downloaded a 1.1 GB crustal anomaly dataset — **never thought to ask whether the animals actually go to the places we were studying.**

It took a human thinking about whales as *intelligent animals*, not data points in a spreadsheet, to notice the most basic flaw in the experimental design.

## What This Actually Means

Here's the pattern across the whole project:

- **Round 1 (2025):** AI fabricates data. Human needs to catch bad numbers. *Human fails.*
- **Round 2 (2026):** Better AI catches bad numbers itself. Runs real analysis. Gets real results. *AI succeeds at the technical work.*
- **Round 3 (2026):** Human asks "do whales even go there?" and reveals the whole comparison was flawed. *Human catches what AI can't.*

Each round, the AI got more capable. And each round, the human's contribution shifted *upward*. The AI doesn't need you to check arithmetic anymore — it does that itself now. But someone still needs to ask **"are we even studying the right thing?"**

That's not a technical skill. You don't need a PhD in marine biology. You just need to think about the problem like a person — with common sense, with empathy for the actual animals involved, with the kind of lateral thinking that comes from caring about the *meaning* of what you're doing, not just the execution.

**AI is getting incredibly capable. And that's making human judgment *more* important, not less** — because the questions that matter keep shifting to a higher level.

## Three Things Worth Remembering

**Start projects now. Finish them later.** AI capabilities are improving fast. A project that's frustrating today might be tractable in six months with better tools. Plant seeds now, come back when the tools catch up. Start the side project even if you can't finish it yet.

**Follow the fun.** The original hypothesis was dead after 8 null results. We almost stopped. The pivot to "can we actually help whales?" came from following curiosity, not a research plan. The risk model — the most interesting outcome — wasn't planned at all. In my experience, AI does its best work when you're both genuinely interested in the problem.

**Change the question, don't refine the answer.** We spent hours testing magnetic gradients at different resolutions. All null. The breakthrough came when we asked a completely different question. If you're stuck, don't keep polishing the same hypothesis. Ask something new.

## Try It Yourself

The whole project — code, data, journal entries, 15 experiment logs — is open source at [github.com/ryan-endacott/madscience](https://github.com/ryan-endacott/madscience).

Every data source we used is free:
- **[NOAA ERDDAP](https://coastwatch.pfeg.noaa.gov/erddap/)** — SST, chlorophyll, bathymetry. No auth needed.
- **[ERA5](https://cds.climate.copernicus.eu)** — 35 years of global wind data. Free registration.
- **[EMAG2v3](https://www.ngdc.noaa.gov/geomag/data/EMAG2/)** — Crustal magnetic anomaly data. Free download.
- **ppigrf** — `pip install ppigrf`. Computes geomagnetic models locally using published coefficients.

There's a methodology framework in the repo called **Scientific AI Sprints** — test hypotheses against existing public data before designing expensive experiments. The whale study is its first case study, including everything that went wrong.

The whale stranding mystery isn't solved. But we know some things that don't cause them, we have a prototype that might help predict them, and we learned something important about how humans and AI work together.

Is this "real science"? A real marine biologist would have caught the control site problem in five seconds. We didn't pre-register hypotheses, we have n=11 events, and the most sophisticated finding is "strandings happen in summer." So... maybe not. But we used real data, real statistical tests, documented every mistake, and caught our own errors in public. I think there's something here, even if it's more "enthusiastic amateur with powerful tools" than "rigorous research." And honestly, I think that's the point — these tools make it possible for anyone to ask real questions of real data. You'll make mistakes. You'll learn. The data doesn't care about your credentials.

*Total cost: ~$200/mo for Claude Max. $0 for data. The control sites need better selection. The whales are still stranding.*
