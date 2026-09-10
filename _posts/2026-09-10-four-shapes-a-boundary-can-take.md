---
layout: post
title: "Four Shapes a Boundary Can Take: Diamond, Hourglass, Bytestream, and Many Small Hourglasses"
date: 2026-09-10
tags: [gutenberg-semantic, diamond, hourglass, cdn, staleness, resolver, geometry]
level: technical
description: "The hourglass has one narrow waist everything passes through — decoupled, but with a single point of dependency. A paged bytestream has no waist at all, every unit independently addressable in parallel. CDNs and multi-source storage are a third shape neither name quite covers: many small hourglasses sharing one interface, resolved fresh per request, paying staleness as the honest cost of that freedom."
---

[The diamond](https://rinie.github.io/2026/08/25/the-diamond-shows-up-eventually/) fuses structure and physical layer at the widest point, so every change touches both sides at once. [The hourglass](https://rinie.github.io/2026/08/25/the-diamond-shows-up-eventually/) fixes that by funnelling everything through one narrow, stable waist. Those two shapes get most of the attention in this series, but they're not the only two, and the other shapes are worth naming precisely rather than folding into "hourglass" by default.

---

## The Hourglass Has a Single Point of Dependency

Worth stating plainly, because it's usually treated as a pure win: an hourglass decouples both sides, but everything still passes through one place. A query optimiser, a DNS resolver, `malloc`'s two function names — if that one waist goes down, everything stops, because everything had to route through it. The decoupling is real. So is the fact that there's exactly one thing in the whole system that isn't allowed to fail.

---

## A Paged Bytestream Has No Waist at All

[Byte-range addressing](https://rinie.github.io/2026/08/29/one-lookup-per-book/) solves the single-point problem differently — not by making the waist more reliable, but by removing it. Every page, every byte range, is independently addressable at the same fine granularity, with no central point anything has to pass through to reach any other point. Losing access to one page doesn't touch any other page's availability. That's not a pinch with one narrow neck. It's closer to a comb — many independent teeth, each reachable on its own, no coordination required between them.

The trade this shape makes, quietly: each page still has exactly *one* authoritative source. Ask for byte range 4000–8000 and there's one place that answer comes from. Parallel addressability, singular truth.

---

## Many Small Hourglasses, One Shared Interface

CDNs and multi-source object storage are a third shape, and it's genuinely different from both of the above rather than a variation on either. [S3, R2, and StackIT all speak the identical interface](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/) for the same logical object — a CDN edge, an S3 origin bucket, an R2 mirror can all hold the same page, and when a request comes in for a specific byte range, something decides *live, per request* which physical copy actually answers: the edge cache if it's warm, the nearest origin if it isn't.

That's not one hourglass. It's many small ones sharing a single waist shape — each request gets its own tiny resolution event, narrow interface in, narrow interface out, but which physical source sits behind that particular waist is chosen fresh, independently, request by request. Not "swap providers at migration time," the way the S3-compatibility argument was framed. Resolution happening at the grain of one page, repeatedly, optimised for whoever's fastest or cheapest right now.

---

## Staleness Is What This Shape Pays For Its Flexibility

The paged bytestream and the plain hourglass both get to skip a problem this shape can't avoid: they each only ever have one source of truth per unit, so there's nothing for two sources to disagree about. Many small hourglasses have several candidate sources for the same page, resolved dynamically — which means they can quietly disagree with each other. Origin updates a page; the edge cache is still serving the old version; two requests, seconds apart, get two different honest answers.

That's [the same shape of trade this series keeps finding everywhere else](https://rinie.github.io/2026/09/07/the-best-dumb-wire/) — a real benefit, paid for with a real, nameable cost rather than a hidden one. Staleness isn't a bug in CDN design. It's the disclosed price of resolving per request instead of once, the same way the wasted last page is the disclosed price of fixed-size addressing. The shape that offers the most flexibility is also the only one of the four with a consistency question the other three got to skip entirely.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [The Diamond Shows Up Eventually](https://rinie.github.io/2026/08/25/the-diamond-shows-up-eventually/) on the diamond and the hourglass, and [The Best Dumb Wire](https://rinie.github.io/2026/09/07/the-best-dumb-wire/) on the paged bytestream this post builds a third shape on top of.*
