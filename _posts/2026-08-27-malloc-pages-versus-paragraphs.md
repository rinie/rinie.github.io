---
layout: post
title: "malloc() Is a Paragraph. The Page Is Someone Else's Problem."
date: 2026-08-27
tags: [gutenberg-semantic, malloc, jemalloc, tcmalloc, mimalloc, abi, resolver, commodity]
level: technical
description: "malloc() and free() operate at paragraph granularity — one call per allocation, whatever size the caller asks for. What jemalloc, tcmalloc, and mimalloc actually manage underneath is page granularity — arenas, size classes, memory requested from the OS in bulk. Three different strategies, one ABI, swappable with a link flag, because the interface never had to know about pages at all."
---

`malloc()` and `free()` take a size and a pointer. That's the entire interface, and it's operated at the same granularity since the 1970s: one call per allocation, whatever size the caller happens to want, no batching, no alignment negotiation, no mention of a page anywhere in the signature.

What actually happens underneath is at a completely different scale. jemalloc, tcmalloc, and mimalloc each request large chunks of memory from the operating system via `mmap`, carve those chunks into arenas and size classes, and run thread-local caches to avoid contention — page-granularity bookkeeping, invisible through the two function names the caller ever touches. Facebook adopted jemalloc specifically for its fragmentation behaviour under their workload. Google built tcmalloc for the same reason at their own scale. Microsoft built mimalloc later, optimising differently again. Three genuinely different internal strategies, swappable with an `LD_PRELOAD` or a link flag, zero changes to any code that calls `malloc`.

---

## Paragraph Interface, Page Resource

This is [the same shape as S3's `GET` and `PUT`](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/) operating at the object level while replication and disk placement stay invisible underneath, and [the same shape as a row staying whole through a pipeline](https://rinie.github.io/2026/08/08/use-specialisation/), narrowed only at the point something actually needs a field. The pattern isn't just "keep the interface dumb." It's narrower than that: keep the interface at a *finer* grain than the physical resource backing it, so the coarse-grained work — pooling, batching, aggregation — has room to happen freely on whichever side of the interface turns out to need it.

If `malloc()` forced the caller to think in pages — which arena, which size class, what alignment — no allocator could compete as a drop-in replacement, because each one manages pages differently on purpose. The interface stayed at paragraph scale specifically so the page-scale strategy underneath could vary per implementation, per workload, per decade, without ever touching the two function names everything above it depends on.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [S3 Out-Evolved HDFS by Refusing to Be Smart](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/) on the same commodity-interface pattern at the storage layer, and [The Diamond Moves, It Doesn't Disappear](https://rinie.github.io/2026/08/26/the-diamond-moves-it-doesnt-disappear/) on C's ABI as the interchange layer C++'s never became.*
