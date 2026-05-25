---
layout: post
title: "Moore's Law as an Architectural Principle"
date: 2026-05-30
---

Every engineer has heard the advice: don't optimise prematurely. But there is a subtler version of the same principle that goes mostly unstated, and it matters more over a five-year horizon than any individual performance decision you will make this quarter.

**Write code that can move to the next platform. Accept the 10% overhead. Collect the 2x improvement for free.**

---

## The Iceberg Model

Think of your running infrastructure as an iceberg. The visible tip is your code and its current performance. The submerged mass — the operating system scheduler, the storage driver, the CPU pipeline, the memory allocator, the runtime JIT — is everything that actually determines throughput. You don't control the submerged mass. You didn't write it, you don't maintain it, and it is going to change whether you want it to or not.

There are two ways to respond to this.

The first is to **move the iceberg**: hand-tune your code to the specific shape of the submerged mass right now. Exploit the particular branch predictor in today's Intel chip. Lay out your data structures around the seek geometry of spinning disks. Size your buffers to the specific latency profile of last year's network stack. This approach can recover the 10% overhead and then some — today.

The second is to **move to a different iceberg**: keep your code at an abstraction level where the submerged mass is someone else's problem to improve. Accept the 10% overhead as the cost of portability. Wait for the next iceberg to arrive.

The second approach sounds passive. It is not. It is a deliberate investment in an option you have not yet exercised.

---

## The 10% Is Not a Tax. It Is a Premium.

When you abstract over storage, over CPU specifics, over memory layout — you are buying the right, but not the obligation, to move to the next platform when it arrives. The option has value precisely because **you do not know which iceberg comes next**.

Apple switched processor architectures twice in twenty years. Nobody predicted the specific shape of Apple Silicon from a 2015 vantage point. Spinning disk gave way to SSD, then NVMe, with each transition inverting the entire cost model of storage access. Random I/O went from expensive to essentially free. Code tuned to minimise random access — carefully, expensively, correctly for its era — became wrong by a factor of a hundred.

Code abstracted over storage geometry collected the NVMe dividend automatically. The 10% overhead it carried in 2008 paid back several times over by 2015, without a rewrite.

This is Moore's Law reframed not as a hardware roadmap but as an **architectural principle**: the underlying platform improves by roughly 2x every two to three years across multiple dimensions simultaneously — compute, memory bandwidth, storage latency, network throughput. Code that is portable to the next platform captures that improvement. Code that is tuned to the current platform does not.

---

## The Option Basket

The improvement is not single-axis. You are holding a basket option on an unknown set of underlying advances:

- The next CPU generation
- The next storage tier
- The next compiler optimisation pass
- The next runtime improvement
- The next language VM — faster garbage collection, better JIT, removed bottlenecks

You do not know which legs of the basket will pay off, or when. The 10% premium buys exposure to all of them. A locked-in optimisation is a short position on the specific thing you optimised for: when it is superseded, the optimisation becomes noise or worse, a constraint that prevents migration.

The SSD transition is the cleanest example because the direction of change was a complete reversal, not a linear improvement. Code that was *correct and optimal* for spinning disk actively *resisted* the NVMe transition — not because it was badly written, but because it was too well-fitted to the wrong iceberg.

---

## Portability Is the Investment

The five-year horizon is the key parameter. At one year, the abstraction overhead looks like unnecessary cost. At five years:

| | Optimised and locked in | Portable, 10% overhead |
|---|---|---|
| Year 0 | 100% | 90% |
| Year 2 | 100% — new platform exists, you cannot use it | 180% |
| Year 5 | 100% — possibly deprecated | 360%+ |
| Rewrite cost | High — every platform shift is your problem | Low — absorbed by the iceberg below you |

The locked-in number does not stay at 100%. It often regresses, because the specific iceberg it was tuned to gets cold. Platform-specific intrinsics from 2010 now carry maintenance cost and limit compiler freedom. Storage layouts tuned to HDD geometry now fight the page cache. The optimisation that was an asset becomes a liability.

---

## Where the Waterline Is

This is where the Gutenberg 2.1 framing is useful. Every system has two layers:

The **Gutenberg layer** is physical and positional: bytes, blocks, CPU cycles, disk sectors, clock ticks. It changes on hardware cycles you do not control. It is the submerged mass of the iceberg.

The **Semantic layer** is logical and named: your data model, your interfaces, your business logic. It changes on business cycles you *do* control.

The waterline between them is the abstraction boundary — and locating it correctly is the architectural decision that determines whether you pay the 10% once or pay a rewrite tax every two years.

Code that reaches *below* the waterline — into the Gutenberg layer — is tuned to the current iceberg. It exploits the specific geometry of today's hardware. It is not wrong. It is just fragile to the timeline.

Code that stays *above* the waterline lets the Gutenberg layer below it improve freely. When Apple changes the processor, when NVMe replaces SSD, when a new allocator halves malloc overhead — the semantic layer above the waterline does not change. You get the improvement for free.

Gutenberg 2.1 — bytestream plus UTF-8 plus git — is itself an example of this principle applied to text and versioning. The A4/Letter distinction is a Gutenberg-layer detail. A document that encodes its meaning in logical structure — headings, sections, paragraphs — can be reflowed to any page size. The A4/Letter distinction stays O(1) regardless of how many documents you have, because it is resolved at render time by the Gutenberg layer, not baked into every paragraph.

The same principle scales. Your data pipeline does not need to know whether the query runs on an 8-core laptop or a 64-core server. Your storage layer does not need to know whether the underlying medium is NVMe or object storage. Your interface does not need to know which processor Apple will ship in 2028.

Locate the waterline. Stay above it. Pay the 10%. Collect the 2x every few years.

The iceberg will move. The question is whether your code moves with it.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) on the boundary between physical and logical layers and what it means for software that needs to survive contact with time. The companion post [The Compiler That Knew Where It Was](https://rinie.github.io/2026/05/31/compiler-snr-evolution/) covers the same waterline idea at the compiler internals level.*
