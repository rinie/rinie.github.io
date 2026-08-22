---
layout: post
title: "Reading the Sand at Different Tides"
date: 2026-09-04
tags: [gutenberg-semantic, waterline, tides, pattern-recognition, curiosity]
level: general
description: "Low tide exposes the sea bed. High tide covers it back over. Walk the same stretch of shoreline twice and you find two different things, not because the first walk was wrong but because the tide was different. Enough walks and pattern recognition stops requiring effort. Most of what turns up, though, isn't new ground at all — it's someone else's footprints, left years earlier, worth following for a while."
---

The most literal thing a real waterline does is change what's visible depending on when you walk it. Low tide exposes the sea bed — the part that's usually underwater, usually hidden, usually [the Gutenberg layer nobody's looking at because the surface is what everyone sees](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). High tide covers it back over. Same shoreline, same walk, two different things found, and neither walk was the wrong one.

---

## The Same Stretch, Twice

This series has walked past XML more than once. [The first pass found loudness](https://rinie.github.io/2026/07/27/dont-hide-the-fence/) — angle brackets shouting louder than the content they were supposed to carry. [A later pass, low tide, found something the first walk never touched](https://rinie.github.io/2026/08/26/the-diamond-moves-it-doesnt-disappear/): the schema itself as the diamond, structure and content fused so tightly that adding one relationship breaks every downstream parser. Neither reading was wrong. The tide was different — enough distance, enough other material walked past in between, that a second look at the same stretch of sand turned up something the first look genuinely couldn't have found.

C++ got the same two passes. One walk found the opaque handle as the fix — PIMPL, a stable pointer plus accessor functions, keeping the struct layout free to change. A different walk, checking a citation properly rather than trusting a half-remembered paraphrase, found the actual reason C's ABI became universal and C++'s never did — not object models smuggling semantics into layout, but a calling convention that was never standardised across compilers in the first place. That correction only happened because the tide went out far enough to expose ground the earlier pass had walked straight over.

malloc got three passes in close succession, each one lower tide than the last: first the two function names hiding page-granularity arenas, then Android's actual 2024 rollout of 16KB pages as a live instance of the same pattern, then UTF-8 giving up character indexing specifically because nobody printing a page ever needed it. Same beach. Every low tide found something the last one hadn't uncovered yet.

---

## Learning to Read the Sand

Enough walks and the reading itself changes. Early on, noticing that an interface stayed thin while its implementation varied underneath took actual looking — a deliberate stop, a specific question asked of a specific example. A hundred-odd posts in, the shape starts announcing itself before the question gets asked. A hardcoded assumption. A structure exposed as an API. A boundary that lets one side move while the other holds still. The pattern doesn't need hunting for anymore. It's just what the sand looks like now, the way a beachcomber stops consciously scanning for shells and starts simply noticing them, peripheral vision doing work that used to take direct attention.

That's not the same as having seen everything. It's closer to having learned what the tide does — which stretches run shallow and reveal a lot on a good low tide, which run deep and stay covered most of the time, which shapes tend to wash up together and which ones are rare enough to be worth stopping for specifically when they do. [Worse ages better than perfect](https://rinie.github.io/2026/06/28/worse-ages-better-than-perfect/) was itself a shape spotted early and then recognised again and again at every tide since — S3 beating HDFS, Python 3's survivable bridge beating Perl 6's unshipped rewrite, a stroll outlasting a march. The pattern wasn't discovered once and then applied everywhere on faith. It kept getting rediscovered, at low tide, on ground that looked different every time, until noticing it stopped requiring effort.

---

## Other People's Footprints

A stroll is rarely the first one down a given stretch. Most of what turns up isn't new ground — it's someone else's tracks, left years or decades earlier, worth following for a while before wandering off in a different direction. Joel Spolsky walked the boundary between what a program does and what a user expects it to do, in 2000, long before this series had a name for either side. Jakob Nielsen walked the same stretch from a different angle, twice — once on how people read, once on who actually participates — and both walks left tracks worth stepping into rather than retracing from scratch. Paul Ford, Taylor Troesh, Damien Katz, Peter Scargill: different decades, different starting points on the shore, all of them noticing something on the same beach this series keeps walking.

None of them were consulted on purpose, the way a citation is chosen to support an argument already decided. They were found the way footprints are found — by walking, noticing someone had been there first, and following for a while to see where the tracks led before setting off again. That's most of what strolling actually produces. Not new territory, most of the time. Older tracks, spotted at the right tide, leading somewhere worth going.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Strolling the Waterline](https://rinie.github.io/2026/09/02/strolling-the-waterline/) on the disposition that makes repeated passes possible, and [Watch Your Step](https://rinie.github.io/2026/09/03/watch-your-step/) on what the same ground can cost you if you stop looking down.*
