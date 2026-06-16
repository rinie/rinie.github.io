---
layout: post
title: "Only Time Will Tell: Evolution Is Cleverer Than You Are"
date: 2026-07-19
tags: [gutenberg-semantic, evolution, gall, shirky, evolvability, waterline]
level: general
description: "A complex system designed from scratch never works and cannot be made to work. Centrally designed protocols start strong and improve logarithmically. Evolvable protocols start weak and improve exponentially. The crossover point is where evolution will tell who suxed less — but only time will tell, because the demo always favours the system that hasn't crossed over yet."
---

A complex system that works is invariably found to have evolved from a simple system that worked. The inverse also appears to be true: a complex system designed from scratch never works and cannot be made to work. You have to start over, beginning with a working simple system.

John Gall wrote that in 1975, in a book about bureaucracies and biological systems, not software. The law is general. It applies anyway.

Evolution is cleverer than you are. That is Orgel's Rule, from molecular biology, by way of Clay Shirky's writing on open protocols. Shirky sharpened it into something with an actual shape: centrally designed protocols start out strong and improve logarithmically. Evolvable protocols start out weak and improve exponentially.

Two laws, two different fields, the same observation arriving from opposite directions. The series has been circling this observation since post 1 without naming the mechanism. Here is the mechanism.

---

## The Shape, Not Just the Sentiment

"Evolution will tell who suxed less" is the closing line the series has used more than once. It sounds like a hope. Shirky's formulation makes it a curve.

**Logarithmic** (centrally designed): big gains early — the designed system is good on day one, because someone thought hard about it before anyone touched it — and diminishing gains after, because every improvement requires revisiting the central design. The committee that wrote the spec has to reconvene for every fix.

**Exponential** (evolvable): small or negative gains early — the evolvable system looks worse than the designed one at first, because nobody centrally guaranteed it would work — but the gains compound, because every user, every fork, every deployment adds capability without needing the original designer's permission.

**The crossover point is the whole game.** Early on, the designed system wins the demo. Late, the evolvable system has lapped it. Every comparison this series has drawn is a claim about which side of the crossover point you happen to be standing on when you judge.

---

## Unix Versus Multics

Multics was centrally designed and strong on day one — a more capable operating system than anything before it, in 1969, by design, with the resources of MIT, Bell Labs, and General Electric behind it.

Unix was Ken Thompson's "simplified Multics," running on a PDP-7 nobody else in the building wanted, written because the simpler thing was what he could actually get working. Weaker on day one. Visibly so.

Fifty years later, Unix's descendants run almost everything that computes — phones, servers, the watch on your wrist, the router in your hallway. Multics ran on a handful of machines, ever, and stopped. Logarithmic versus exponential, played out to its conclusion before anyone had named the curve.

---

## Git Versus Centralised Version Control

Shirky told this story himself, at TED, about the system he was best positioned to explain because he had watched it happen. Centralised version control existed to prevent people from inadvertently — or deliberately — breaking the shared codebase. One owner, many workers, permissions checked at every step. It is feudal, and it is entirely appropriate for a commercial software company with one codebase and one chain of command.

Linus Torvalds wanted something different: everyone has access to all of the source code, all of the time. No central permission gate. The chaos that this would obviously cause is held in check by something almost embarrassingly simple — a cryptographic signature that gives every change a unique, verifiable identity. Cooperation without coordination.

Git looked unnecessary and chaotic on day one. Why would every contributor need the full history? Centralised systems already solved the obvious problem. CVS and SVN were strong early, because they solved exactly the problem everyone could see.

The exponential curve is the reason GitHub exists and SourceForge does not.

---

## Matter Versus "It's Just on the LAN"

Matter is centrally designed, strong on the whiteboard — one standard, every device, by committee, with the full weight of Amazon, Apple, Google, and the Connectivity Standards Alliance behind it. The commissioning ceremony from [an earlier post](https://rinie.github.io/2026/07/17/wifi-browser-router-evolve/) is the logarithmic tax made visible: every improvement to the onboarding experience requires the standards body to revisit the design, ratify it, and wait for every vendor to implement the new version.

mDNS plus a local HTTP API is evolvable and visibly weaker on day one — no unified onboarding flow, every device doing its own slightly different thing, nobody guaranteeing it will all fit together. RFLink, Domoticz, and Home Assistant keep compounding anyway, because no central body has to approve the next improvement before it ships.

Which one wins is still being decided. That is the honest answer, and it is also the point — the crossover has not happened yet, if it is going to happen at all.

---

## HTML Versus XML

XML was centrally designed and strong by specification — well-formed, strict, validating, mathematically clean. XHTML tried to bring that rigour to the web.

HTML5's error-handling model is the opposite stance: be liberal in accepting, even tag soup, even markup nobody intended to be parsed by a machine. Weaker by spec. The browser has to guess what the author meant, and different browsers used to guess differently, which is exactly the muddiness a strict specification was supposed to prevent.

HTML is what took over the planet. The logarithmically-improved, rigorously specified successor did not.

---

## Why the Logarithmic System Looks Like It Is Winning

This is the part worth stating carefully, because it is the actual trap hiding inside the whole argument: the logarithmic curve is not lower everywhere. It is higher at the start. Every "evolvable beats designed" story in this series is also a story about someone betting correctly on a system that looked worse, for long enough that most reasonable observers would have bet the other way.

This is the same shape as the 90% signal arriving slowly, described in earlier posts. The designed system's strength is front-loaded and visible immediately — the demo, the launch event, the whitepaper, the keynote. The evolvable system's strength is back-loaded and only visible in aggregate, after enough forks, enough users, enough small iterations have compounded into something the demo could never have shown on day one.

At any given moment before the crossover, the centrally designed system has the better demo. The exponential curve does not announce itself. It just keeps compounding, quietly, until one day the comparison that used to be obvious is not obvious anymore.

---

## Only Time Will Tell

"Evolution will tell who suxed less" is correctly phrased in the future tense, not the past. Gall's Law and Orgel's Rule explain the mechanism. They do not supply the answer early. They supply the patience required to wait for it.

The system that is currently winning the demo is not necessarily the system that will be running everything in fifty years. The system that currently looks unnecessary, chaotic, and weaker than its rival might be the one quietly compounding underneath the place nobody is looking.

You cannot know which curve you are on by looking at today's snapshot. You can only know by waiting — and by noticing, this time, that the weaker-looking option might be the one with room to grow.

Only time will tell. Evolution is cleverer than you are.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Worse Is Better Because the Gap Is Where Evolution Happens](https://rinie.github.io/2026/06/01/worse-is-better-gap-evolution/) on Gabriel's original formulation, [The Watershed](https://rinie.github.io/2026/07/02/the-watershed/) on the slow-then-sudden shape of platform transitions, [Embracing the Web Instead of Fighting It](https://rinie.github.io/2026/07/13/embracing-the-web/) on XML losing to HTML, and [WiFi, the Browser and the Router Just Evolve](https://rinie.github.io/2026/07/17/wifi-browser-router-evolve/) on the closing line this post finally explains.*
