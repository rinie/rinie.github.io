---
layout: post
title: "In Praise of Dumb Wires: Reliable, Adds Nothing, Wastes Nothing, Swappable"
date: 2026-09-05
tags: [gutenberg-semantic, commodity, resolver, malloc, s3, http, reliability]
level: general
description: "An AI assistant, asked to reframe this series' 'dumb wires' as something more palatable, offered 'commodity infrastructure' — accurate, but softer than the original claim needed to be. Dumb isn't a euphemism problem. It's four separate virtues, each earned, each lost the moment an interface tries to be clever — including a bounded waste that explains why file-per-chapter beats both file-per-book and file-per-paragraph."
---

Asked to rephrase "dumb wires" into something more palatable, an AI assistant offered "commodity infrastructure" — accurate, and immediately softer than the claim needs to be. Commodity describes an outcome: an interface enough vendors implement that it becomes swappable. Dumb describes the cause. An interface can be popular without being dumb — plenty of proprietary, opinionated APIs have plenty of users. What makes a wire worth trusting isn't that it's common. It's that it doesn't try to be smart. Four separate virtues fall out of that refusal, and each one is worth naming plainly rather than laundering into something that sounds nicer in a boardroom.

---

## Reliable

A dumb wire has fewer ways to be wrong because it does less. `malloc()` and `free()` haven't needed to change their signature since the 1970s — two function names, no embedded logic about what's being allocated or why, nothing to have a bug in beyond the allocation itself. [HTTP's Range header](https://rinie.github.io/2026/08/29/one-lookup-per-book/), specified in 1999, still means exactly what it meant then, because it never tried to know anything about what the bytes on the other end of the request actually were. Every decision an interface doesn't make is a decision that can't later turn out to be wrong.

---

## Adds Nothing

S3's `GET` and `PUT` don't care whether the bytes are a JPEG, a database backup, or a Parquet file. That indifference is the entire point — [the meter measures kilowatt-hours, not what's plugged in](https://rinie.github.io/2026/08/03/the-meterkast-pattern/). An interface that adds nothing of its own leaves the whole meaning-making job to whoever's actually equipped to do it, on either side of the wire, instead of imposing a half-informed opinion in the middle where nobody asked for one.

---

## Wastes Nothing — Beyond a Known, Bounded Cost

This one needs the honest version, not the marketing version. [A4 wastes part of the last page](https://rinie.github.io/2026/08/28/the-page-size-can-evolve-too/). Disk sectors moving from 512 to 4096 bytes paid a real read-modify-write tax at exactly the seam between old and new. A dumb wire does waste something — but the waste is visible, predictable, and bounded in advance, which is a different claim than "wastes nothing" and a stronger one. A smart wire's cost isn't bounded at all. Nobody can say in advance how expensive a wrong embedded assumption will turn out to be once it finally breaks, because the cost was never priced in — it was hidden inside cleverness that looked free until the day it wasn't.

The bound is worth stating precisely, because it explains a real design choice this series hasn't made explicit yet. A book of 15 pages and a book of 150 pages waste the same amount — less than one page, once, at the end of the file. The waste attaches to the physical container, not to how much content is inside it, [the same anchor point as one DNS lookup per book](https://rinie.github.io/2026/08/29/one-lookup-per-book/) regardless of how many pages follow it.

That fixed, per-file cost is what actually decides the right granularity for splitting a book into separate files at all. File per book wastes almost nothing but loses independent addressability — touching chapter three means touching the whole book. File per paragraph — the semantic Post-it, a UUID or an RDF triple for every atomic fact — maximises addressability and pays the fixed waste on every single unit, which for a one-sentence paragraph can mean the overhead is larger than the content it's attached to. File per chapter sits in the middle for a reason: coarse enough that the fixed cost stays a small fraction of what's actually stored, fine enough that a chapter is still independently reachable without dragging the rest of the book along. The granularity that wins isn't the finest one available. It's the one where the bounded waste this series keeps finding stays genuinely small relative to what it's the waste of.

---

## Swappable

Everything else follows from the first three. An interface that's reliable, adds nothing, and wastes only what it openly admits to wasting is exactly the interface a stranger can reimplement faithfully — [which is the entire reason R2 and StackIT could compete with S3 without asking a single customer to change a line of code](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/), and the entire reason jemalloc, tcmalloc, and mimalloc can all sit behind the same two function names. Swappability isn't a feature bolted onto a dumb wire afterward. It's what dumb, done honestly, buys you for free.

---

Four virtues, none of them softened, none of them needing a rebrand to be worth wanting. "Commodity" describes what a dumb wire eventually becomes once enough people trust it. It was never the reason to build one in the first place.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [S3 Out-Evolved HDFS by Refusing to Be Smart](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/) on the original case for staying dumb, and [The Meterkast Pattern](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) on the meter that measures without interpreting.*
