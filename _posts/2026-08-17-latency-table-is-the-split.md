---
layout: post
title: "The Latency Table Already Knows the Gutenberg Layer Compounds and the Semantic Layer Doesn't"
date: 2026-08-17
tags: [gutenberg-semantic, latency, moores-law, availability-zones, cdn, resolver, speed-of-light]
level: technical
description: "Jeff Dean's 2012 latency table has aged unevenly — SSD random reads got 10x faster, memory bandwidth got 15x faster, but a packet from California to the Netherlands still takes 150ms. That unevenness isn't decay. It's the Gutenberg/Semantic split, measured: technology compounds, physics doesn't, and the one number that never moves is exactly why cloud regions exist."
---

[The classic latency table](https://colin-scott.github.io/personal_website/research/interactive_latency.html) — L1 cache reference, disk seek, packet to the Netherlands and back — has aged unevenly over the thirteen years since Jeff Dean first circulated it. Some lines are badly out of date. Others are exactly as true today as they were in 2012. That unevenness isn't the table decaying. It's the table accidentally proving the split this series keeps finding everywhere else.

---

## Two Kinds of Number, Aging at Two Different Rates

Sort the table's entries by what actually bounds them, and the pattern is immediate. **SSD random read**, quoted at 150 µs in the original, reflects 2012-era SATA SSDs; a modern NVMe drive does the same read in roughly 15 µs — ten times faster. **Reading a megabyte sequentially from memory**, quoted at 250 µs, is bounded by memory bandwidth, which has also moved: modern DDR5 puts the same read closer to 15 µs, another order of magnitude gone. Both numbers were bound by *technology* — how fast engineers could build a drive controller, how many bits per second a memory bus could push — and technology compounds. Every year, someone ships a faster part, and the number quietly drops.

**A packet from California to the Netherlands and back**, quoted at 150 ms, is exactly 150 ms today. It hasn't moved, and it structurally cannot move by more than a rounding error, because it's bound by the speed of light in fiber and the physical distance between two points on a sphere. No engineering effort compounds against that constraint. You can lay a straighter cable. You cannot make light go faster.

That's the Gutenberg/Semantic split, laid out as two different curves on the same table. The Gutenberg layer — the physical implementation, the drive controller, the memory bus — is exactly [the layer this series has always said compounds](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/), improving on something like a Moore's Law curve because it's made of engineering choices that keep getting better. The distance from California to the Netherlands isn't an implementation choice. It's a fact about the shape of the Earth, sitting underneath every implementation, refusing to compound no matter how many decades pass.

---

## The Floor Is the Reason Regions Exist

This is also, without any extra argument required, the entire justification for [cloud regions and CDN edge nodes](https://rinie.github.io/2026/07/28/at-sign-layer-boundary/). Once the SSD gets fast enough and the memory bus gets wide enough, the only latency left standing between a user in Amsterdam and a server in California is the one number in the whole table that engineering cannot touch. You can't out-optimise 150 ms. You can only stop asking the packet to make that trip.

That's exactly what a resolver serving the nearest copy does. [Cloud infrastructure doesn't move Site, it multiplies it](https://rinie.github.io/2026/08/04/cloud-breaks-the-pace-layers/) — a copy of the service gets provisioned in a European region, an anycast DNS answer routes the Dutch user to the nearby copy instead of the distant one, and the 150 ms floor simply never gets paid, because the request never crosses the ocean in the first place. The technology-bound numbers in the table get faster on their own, year over year, without anyone deciding to build a new region. The physics-bound number never will, which is precisely why "put a copy near the user" remains the only lever anyone's ever found for it, from the first CDN edge cache to the newest cloud availability zone.

---

## A Table That Should Be Read as Two Tables

The practical upshot for anyone still citing the 2012 numbers from memory: split the table before trusting it. Anything bound by a physical constant — the speed of light, the number of clock cycles in a cache hit — is safe to keep citing indefinitely, because it was never going to move. Anything bound by a specific piece of hardware's throughput needs a fresh number roughly every few years, because that's the part of the table living on the Gutenberg layer's compounding curve, and nobody should expect a thirteen-year-old figure to still describe current silicon.

The table was never wrong. It just mixed a constant and a variable in the same list, without labelling which was which — and thirteen years later, the unlabelled variable has moved by an order of magnitude while the unlabelled constant, predictably, hasn't budged at all.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Cloud Breaks the Pace Layers](https://rinie.github.io/2026/08/04/cloud-breaks-the-pace-layers/) on multiplying Site instead of moving it, [Wirth's Law Only Holds If You Recompile the Whole Iceberg](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/) on which layers compound and which don't, and [URL, DNS and the @ Sign](https://rinie.github.io/2026/07/28/at-sign-layer-boundary/) on the resolver that picks the nearest copy.*

Source: [Latency Numbers Every Programmer Should Know](https://colin-scott.github.io/personal_website/research/interactive_latency.html), originally compiled by Jeff Dean and Peter Norvig, interactive version by Colin Scott.
