---
layout: post
title: "One DNS Lookup Per Book. Then Pages, However They Show Up."
date: 2026-08-29
tags: [gutenberg-semantic, dns, cdn, caching, multicore, latency, resolver, nested-resolution, utf-8, self-synchronizing]
level: technical
description: "A URL resolves to an IP once, coarse and infrequent — one lookup for the whole book. Everything after that is pages, each with its own resolution, hitting whatever cache tier answers fastest. UTF-8 proves exactly which granularity actually matters: it gives up O(1) character indexing entirely, keeps self-synchronizing byte offsets, because a printer asking for page two was never going to ask for character 5,000."
---

A URL resolving to an IP address is a coarse, infrequent event — once per connection, or once per DNS TTL, picking which server you're even talking to. That's a proper hourglass waist: narrow, stable, and it doesn't move again until it has to. Everything that happens after it is a different kind of resolution entirely, running at a completely different grain, and it's worth being precise about the difference rather than treating "resolve the request" as one event.

---

## The Book Gets One Lookup. The Pages Get Many.

Fetching a document isn't one resolution — it's one coarse resolution followed by many fine ones, nested underneath it. DNS decides which server. Every individual byte-range request after that gets its own independent answer: is this page hot in an edge cache's memory, warm on local disk, or does it have to go all the way back to origin? [That's exactly what the latency table measures without naming the geometry](https://rinie.github.io/2026/08/17/latency-table-is-the-split/) — the same logical operation, "get this page," landing at wildly different physical tiers, L1 cache through cross-ocean fetch, and the caller's interface never has to know which one actually answered.

One lookup per book. Then pages, however they show up — and "however they show up" is doing real work in that sentence, because it's a different answer on every single request, decided fresh, invisible to whatever asked for the page in the first place.

---

## Multicore Turns One Fetch Into a Burst

Add concurrency and the nesting gets a third dimension rather than a third layer. A single working set — one program reading one logical file — can fire off many of these page-level resolutions at once, each independently landing at a different cache tier, each carrying its own latency, all in flight simultaneously. "Read this file" stops being one resolution event and becomes a scattered burst of dozens of small, parallel, independently-resolved fetches, some answered from a core's local cache line, some from RAM, some from a CDN edge three hops away — and none of them waiting on each other, because none of them ever agreed to be sequential in the first place.

---

## UTF-8 Shows Which Granularity Actually Mattered

The printer needs to print page two. Not a paragraph, not a chapter, not the whole book — a page, defined as a byte range in a table built once, during layout. That's a coarser requirement than it might sound, and UTF-8's own design proves exactly how much coarser.

UTF-8 gives up O(1) character indexing entirely. Unlike UTF-32, where the Nth code point sits at a computable byte offset, UTF-8 characters vary from one to four bytes, so there's no formula for "where is character 5,000" — you'd have to scan. That looks like a real cost, and for some workloads it is. But UTF-8 keeps something narrower and, for pagination, entirely sufficient: it's *self-synchronizing*. Continuation bytes are unambiguously marked, so from any arbitrary byte offset you can recover the nearest valid character boundary with a small, bounded scan — at most three bytes forward. [The faster-cpython team's own reasoning for eventually wanting UTF-8 internally](https://rinie.github.io/2026/08/20/python3-migration-good-destination-questionable/) confirms the shape of the trade directly: O(1) code-point indexing is expensive to fake, and most real work never needed it.

The concrete proof is almost exactly the multicore case above: split a UTF-8 stream across workers by raw byte offset — divide the file into equal chunks, no coordination required — and each worker just seeks forward up to four bytes to land on a genuine character boundary before starting to decode. No shared index. No character-level lookup. Byte-range addressing plus a bounded local scan is enough.

There's a sharper edge to this worth separating out, because it isn't just "the printer doesn't need extra work" — it's "the printer doesn't need a dependency that a sequential design would have invented." UTF-16 and UTF-32 generally need a byte-order mark at the start of the stream to establish endianness before anything else can be decoded correctly, which means every page implicitly depends on byte zero. UTF-8 was deliberately designed not to need this — it's byte-order independent, so decoding page fifteen never requires having read anything about page one at all. The printer doesn't need to know the BOM to print pages fifteen and sixteen, because nothing about those pages was ever actually coupled to the start of the file. The coupling would have been invented by the format, not required by the content.

The same logic rules out the other false dependency a sequential design would create: validate-then-print, where a single encoding error on page eighty-two blocks output of pages that have nothing to do with it. Self-synchronization means a corrupt or unreadable byte range stays a local problem — the decoder finds the next valid boundary and carries on, or reports failure scoped to exactly the page that failed. Pages fifteen and sixteen were never going to depend on page eighty-two's validity in the first place, and a format that forces a whole-document check before it will print anything is manufacturing a dependency the actual structure never had.

That's the whole answer to which granularity actually mattered. The printer was never going to ask for character 5,000 anyway. It asks for page two — a byte range, resolved once, reused every time, independent of byte zero and independent of every other page's condition — and UTF-8's designers gave up exactly the indexing property nobody printing a book needed, while keeping the two properties, self-synchronization and byte-order independence, that pagination, chunking, and parallel decoding all actually depend on.

---

## Why the Interface Stayed This Boring

None of this is exploitable if the interface exposes any of it. A byte-range request that told the caller which cache tier answered, or forced it to know in advance whether a given page would be local or remote, would make dynamic rerouting impossible — every caller would have to be rewritten every time the caching strategy underneath it changed. The interface stays exactly as boring as a page number and a range precisely so the resolution behind it can keep rearranging itself, per request, per core, per cache generation, without a single line of calling code ever needing to notice.

The simplicity isn't happening despite the wild variance underneath. It's what makes the variance safe to exploit at all — one lookup for the book, and everything downstream of it free to get faster, more parallel, and more distributed, decade after decade, because nobody above the page boundary ever had to be told how.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [The Latency Table Already Knows the Gutenberg Layer Compounds and the Semantic Layer Doesn't](https://rinie.github.io/2026/08/17/latency-table-is-the-split/) on the same nested tiers measured directly, [malloc() Is a Paragraph. The Page Is Someone Else's Problem.](https://rinie.github.io/2026/08/27/malloc-pages-versus-paragraphs/) on keeping the interface finer than the resource behind it, and [The Migration Was Good. The Destination Is Still Debatable.](https://rinie.github.io/2026/08/20/python3-migration-good-destination-questionable/) on Python's PEP 393 optimising for the wrong granularity.*

Source: [UTF-8: Bits, Bytes, and Benefits](https://research.swtch.com/utf8), Russ Cox; ["Use UTF-8 internally for strings"](https://github.com/faster-cpython/ideas/issues/684), faster-cpython/ideas, GitHub.
