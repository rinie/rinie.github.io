---
layout: post
title: "Cheap Moves Instead of Silver Bullet"
date: 2026-06-27
tags: [no-silver-bullet, moores-law, pace-layering, gutenberg, portability, brooks]
level: deep-dive
description: "Brooks was right that there is no silver bullet — but he aimed the demand at the layer that could never supply one. The leverage was never a single ten-times leap. It was a cheap, repeatable move at the fast layer, bought as insurance against the next iceberg."
---

In 1986 Fred Brooks wrote that there would be no silver bullet — no single development, in technology or management technique, that by itself promised even one order-of-magnitude improvement in productivity within a decade. Forty years on, he is still right. The bullet never came. Every candidate that was supposed to be it — the new language, the new paradigm, the new framework, the new model — delivered something, sometimes a great deal, but never the clean ten-times leap that ends the difficulty for good.

I want to agree with Brooks completely and then point out the one thing he got wrong. Not the conclusion. The *aim*.

## The demand was pointed at the wrong layer

Brooks located the absence of the silver bullet in what he called *essential* complexity — the irreducible difficulty of the thing itself, the meaning the software has to model, as opposed to the *accidental* complexity of the tools we happen to use to express it. His argument was that we had already killed most of the accidental complexity, so the remaining hard part was essential, and the essential part does not yield to better tools. You cannot buy your way out of the difficulty of the problem.

That is exactly true — at the layer he was looking at. In the vocabulary this series has been building, the essential complexity lives at the **Semantic layer**: the meaning, the domain, the irreducible *what the program is about*. And the Semantic layer genuinely has no silver bullet, for precisely Brooks's reason. Meaning does not get cheaper because your compiler improved.

But there is a second layer underneath the meaning — the **Gutenberg layer**: the commodity substrate of bytes, blocks, pages, the physical-positional stratum that carries the meaning without being it. Brooks demanded a bullet and, finding none at the Semantic layer, concluded there was none to be had. He was looking in the only place a bullet was impossible, and missing the layer where the leverage actually lives. The leverage at the Gutenberg layer is real — it just isn't a bullet. It's a cheap move.

## Two layers move at different speeds

The reason the two layers behave so differently is that they move at different speeds — Stewart Brand's pace layering, the observation that in any durable system the fast layers innovate and absorb shocks while the slow layers stabilise and remember. Fashion moves fast, infrastructure moves slowly, and the layers shear against each other at the boundary.

Software has the same shearing. The Semantic layer — the meaning, the domain model, the hard-won understanding of the problem — moves *slowly*, because understanding a problem is expensive and doesn't depreciate quickly. The Gutenberg layer — the substrate, the encoding, the platform, the hardware — moves *fast*, and keeps moving, because that is where Moore's Law and its successors keep depositing their gifts.

This is why the silver bullet and the cheap move land on different layers. A silver bullet would be a single large change to the *slow* layer — a new way of thinking that suddenly makes the meaning ten times easier. That can't happen on demand, because the slow layer is slow on purpose. A cheap move is a small, repeatable change at the *fast* layer — and the fast layer is exactly where small repeatable changes accumulate. You don't get one leap. You get the next two-times, and then the next, riding a substrate that keeps shifting underneath you.

## What a cheap move actually is

A cheap move has a specific shape, and the shape is the whole series in miniature: **uniform, independent, severable pages.**

Cut a book at a chapter boundary and it costs you — the meaning spans the seam, you can't understand the part without the whole. Cut it at a *page* break and it costs nothing, because the pages are commodity and independent. That severability is what makes a move cheap. When the substrate shifts — new platform, new encoding, new runtime — commodity pages re-home onto it for almost nothing, because nothing essential was fused into where they happened to sit. You swap the implementation below the waterline and the meaning above never notices.

The structures that *can't* move cheaply are the ones that fused meaning into position. A deeply nested chapter hierarchy, an architecture whose value lives in its global shape, a maze of fine-grained semantic fragments — these charge you to cut anywhere, because every seam is load-bearing. They are the opposite of a cheap move: they are a standing bet that the substrate will never change. It always changes.

There's a second dividend that comes free with the same shape. Uniform independent pages *parallelise* — equal-sized chunks, no chunk depending on its neighbour, the exact two properties a SIMD lane, a vectorised column engine, or a partitioned data store needs to spread work across cores. The nested hierarchy and the fragment-maze both fail it: one serialises you by meaning, the other drowns you in pointer-chasing. So the cheap break and the cheap parallel are the same property seen twice. The page that's cheap to *move* is also cheap to *split*.

## The price of the option

None of this is free, and it shouldn't pretend to be — that would just be the silver bullet wearing a different hat. Keeping your pages uniform and your seams severable costs something up front: call it ten percent, the overhead of not fusing meaning into the substrate when fusing would have been locally convenient. (This is the same ten percent I argued for in [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) — there framed as the recurring two-times you collect by staying portable.)

That ten percent is not a productivity gain. It buys you nothing today. What it buys is an *option*: the right, but not the obligation, to move to the next iceberg cheaply when it surfaces. It's insurance, and insurance is supposed to feel slightly wasteful right up until the day it doesn't. The silver bullet was sold as a gain. The cheap move is honestly sold as a premium — small, recurring, and worth it only because the substrate is guaranteed to shift and the structures that fused themselves to the old one will not survive the move.

So the corrected claim is narrow and, I think, true: there is no silver bullet, exactly as Brooks said — *at the layer he was aiming at*. At the layer underneath, there is no bullet either, but there is a cheaper move, and the cheaper move compounds where the bullet only ever misfired.

## Potholes, not moonshots

This has a working style attached, and the clearest living example of it is the way Linus Torvalds runs the kernel: fix potholes, don't chase moonshots.

A moonshot is a silver-bullet bet — one large swing for the order-of-magnitude leap. A pothole fix is a cheap move — small, local, severable, independent of the next one. The thing worth noticing is that the kernel's legendary reliability is not the result of any moonshot. It's the *compounding* of an enormous number of pothole fixes, each cheap, each independent, none of them individually impressive. It's the two-times collected steadily over thirty years, not the ten-times gambled for once and missed.

It's also the constructive face of an instinct that's easy to express badly. Stated as a dislike — *that paradigm is wrong, that abstraction makes you write bad code* — the anti-silver-bullet temperament reads as a flame, an attack on someone's essence. Stated as *what you do instead* — keep the road drivable, never break what works, let the improvement accumulate at the layer where breaks are cheap — it reads as discipline. Same conviction; one version picks a fight, the other just keeps the surface usable. The pothole framing shows the discipline without the disdain.

And it's the quiet rebuttal to "move fast and break things." You don't move fast by breaking — breaking is the expensive cut, the deprecation that declares the old thing dead and forces everyone downstream to follow. You move fast by *not* breaking: ship the new page alongside the old, let the old keep resolving, swap underneath the waterline where no contract can see it. Dual-track, no deprecation. Move fast *without* breaking. That, too, is a cheap move — the old page costs nothing to keep, because it was a commodity page all along.

---

The silver bullet was always a category error: a demand for a single large change at the layer that's slow on purpose. The cheap move is the honest alternative — a small change at the layer that's fast, severable so it can leave, uniform so it can split, bought as insurance against the iceberg that's already moving. It won't make you ten times faster this year. It will keep you free to leave when the substrate does.

There is no silver bullet. There is only a cheaper move.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) on the recurring two-times dividend, [Don't Go Down With Your Iceberg](https://rinie.github.io/2026/06/13/dont-go-down-with-your-iceberg/) on the cost of fusing meaning into the substrate, and [Worse Is Better Because the Gap Is Where Evolution Happens](https://rinie.github.io/2026/06/01/worse-is-better-gap-evolution/) on why the slow layer has no silver bullet.*
