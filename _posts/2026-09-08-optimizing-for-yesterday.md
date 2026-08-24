---
layout: post
title: "Worse Ages Better Because Perfect Is Smug, Optimizing for Yesterday"
date: 2026-09-08
tags: [gutenberg-semantic, worse-is-better, strolling, waterline, hdfs, architecture-astronauts]
level: general
description: "Every optimization is calibrated against data that already exists — there's no way to optimize for a future that hasn't happened yet. Smart is structurally retrospective the moment it ships. Smug is the failure of not knowing that about itself. A stroll never commits to a destination, so it has nothing precise enough to go stale, and nothing to be smug about in the first place."
---

[Worse ages better than perfect](https://rinie.github.io/2026/06/28/worse-ages-better-than-perfect/) was the claim in June. [Strolling the waterline](https://rinie.github.io/2026/09/02/strolling-the-waterline/) makes the same claim again, sharper, three months and a dozen posts later: worse is dumb. Perfect is smart, and smart is smug — confident it has correctly anticipated the general case, when what it's actually done is optimize for yesterday.

That's not a metaphor. Every optimization is calibrated against data that already exists at the moment of calibration — there's no other kind of data available to optimize against. A future that hasn't happened yet cannot be measured, so it cannot be optimized for. Smart is structurally retrospective the instant it ships, no matter how forward-looking it feels while it's being built. Smug is the specific failure of not knowing that about itself — believing the current constraint is the permanent one, and building with the confidence that belief buys.

HDFS colocated storage and compute because 2006-era network bandwidth made that the correct answer, and it was smug enough about the answer's permanence to build a whole ecosystem's assumptions on top of it. [The correction took a decade](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/), arriving exactly when the bandwidth that justified the original bet finally moved. PEP 393 optimized Python's strings for fast code-point indexing, confident that was the operation worth being clever about — smug enough that [twelve years later, the same team is still discussing whether that confidence was ever earned](https://rinie.github.io/2026/08/20/python3-migration-good-destination-questionable/). Every architecture astronaut this series has found was solving the template of a problem with exactly this confidence — that the general case had been correctly seen in advance, which is the one thing optimization can never actually verify about itself.

A stroll doesn't have this problem, and it's worth being precise about why rather than treating it as a nicer disposition. A march commits to a destination and optimizes the route to it — which means a march is always, structurally, betting on the same yesterday-shaped confidence smart infrastructure bets on, just applied to a walk instead of a wire. A stroll commits to nothing beyond the next visible stretch of shore. It has no destination precise enough to optimize toward, which means it has nothing precise enough to go stale, and nothing to be smug about in the first place. That's not a lesser form of walking. It's the only form that doesn't quietly promise a future it has no way of actually knowing.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Worse Ages Better Than Perfect](https://rinie.github.io/2026/06/28/worse-ages-better-than-perfect/), the original claim this post sharpens, and [Strolling the Waterline](https://rinie.github.io/2026/09/02/strolling-the-waterline/) on the disposition that never makes the bet in the first place.*
