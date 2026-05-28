---
layout: post
title: "The Compiler That Knew Where It Was: SNR in Intermediate Representations"
date: 2026-05-31
category: "Software Architecture"
tags: [compilers, intermediate-representations, snr, type-systems, llvm]
depth: deep-technical
---

*This is a technical deep dive. If you are here for the architecture principles, the [previous post](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) is the right starting point. Come back when you want to know why those principles show up even in compiler internals.*

---

In 1988, a team of four German engineers — Herbert Czymontek, Gerd Hildebrandt, Andreas Roth, and Peter Sollich — built a C compiler for the Atari ST that generated code the machine language programmers around them couldn't easily beat by hand. They were working under contract for Borland GmbH in Munich, targeting the Motorola 68000, and what they produced was eventually sold as Turbo C for the Atari ST and then, after Borland exited the platform, continued independently as Pure C.

The compiler had two properties that stood out. It eliminated stack frames for functions that didn't need them. And it allocated string literals by offset from the previous string literal, because it knew at compile time exactly where everything in the function's data segment would land.

Neither optimisation required exotic theory. Both required one thing: **the compiler never threw away the structure of the source code**.

---

## The Newspaper Without Headlines

A modern compiler's standard intermediate representation (IR) is a flat list of basic blocks — labelled sequences of instructions with no branches in except at the top, and no branches out except at the bottom. Control flow is a graph of edges between these blocks.

This representation is mathematically tractable. Dataflow analysis, liveness computation, dominance trees — all the standard algorithms operate cleanly on it. Every compiler textbook since the 1970s teaches it.

It is also, from a signal-to-noise perspective, a newspaper with no headlines.

Every basic block is labelled `bb_0042` or similar. The label tells you nothing about whether you are inside a loop, an error path, a leaf function, or the cold branch of an unlikely condition. The structure that the programmer wrote — the `{}` that said *this is a loop body*, *this is an error handler*, *this is a leaf with no sub-calls* — has been destroyed. To recover it, the compiler runs loop detection, dominance analysis, and call graph inspection. It reconstructs, expensively, the index it threw away for free at the front door.

The 1988 Atari ST compiler never made this mistake. It operated at scope level throughout. A `for` loop stayed a for loop until code emission. A leaf function — one with no sub-calls and locals that fit in registers — was *known* to be a leaf function from the moment the parser closed its `}`. No analysis required.

---

## What Structure Gives You for Free

The two Turbo C optimisations fall out directly from keeping the structure.

**Stack frame elimination.** On the 68000, a standard function prologue and epilogue looks like:

```
link  a6, #-n      ; 4 bytes, 16 cycles — establish frame pointer
...
unlk  a6           ; 2 bytes, 12 cycles — tear it down
```

For a leaf function whose locals fit in data registers, this is pure overhead. The compiler needs to know three things to eliminate it: the function makes no sub-calls, all locals fit in registers, and nothing takes the address of a local. In a flat basic-block IR, recovering this knowledge requires liveness analysis across the entire function graph plus a call graph query. In a scoped IR, it is a structural property visible at the closing `}`. You know it before you emit a single instruction.

**String literal offset optimisation.** Two string literals in the same function will be emitted adjacently in the data segment. A reference to the second can be expressed as an offset from the first — saving a relocation entry and a register load in the emitted code. In a flat IR, string literals are anonymous globals with no topological relationship to each other. Recovering adjacency requires a separate interning and layout pass. In a scoped IR, both strings are *in the same scope*, which means the compiler knows their relative layout before the linker is involved.

Both optimisations are instances of the same principle: **information that was free at the source level costs an analysis pass to recover at the IR level**. If you never destroy the information, you never pay to reconstruct it.

---

## The SNR Framing

Signal-to-noise ratio in an IR is not about verbosity. It is about how much of what you read is *load-bearing* versus *compensating for a previous information loss*.

A flat basic-block IR has a high noise floor because every branch, every phi node, every `br label %bb_0042` is there to express something the source already expressed as `if`, `while`, or `{}`. The edges in the control-flow graph are the reconstruction cost of the structure that was flattened.

LLVM IR is the extreme case. It is expressive, correct, and well-engineered. It is also genuinely difficult to read, not because of the C++ that surrounds it, but because the IR itself carries the overhead of having destroyed its own structure. Reading LLVM IR to understand what a function does requires mentally reassembling the `{}` hierarchy from a graph of labelled blocks. You are doing by eye what the loop detection pass does algorithmically.

The C++ OOP that wraps LLVM is partly genuine complexity and partly **load-bearing scaffolding for managing the graph complexity the flat IR created**. Remove the flat IR and a significant fraction of the scaffolding becomes unnecessary.

---

## The Wheel Turning Back

Three modern projects are rediscovering, from different directions, what the 1988 Munich team knew by constraint.

**Zig's AIR.** Andrew Kelley made the deliberate choice to preserve structured control flow through the compiler's analysis phases. Zig goes AST → ZIR → AIR (Analyzed IR) → MIR, and the scope structure survives intact through AIR. Each function is analysed as a unit with its structural invariants intact. MIR — machine IR — is where the structure finally flattens, at the last possible moment. This is exactly the Turbo C approach: keep the index until you are actually printing the newspaper.

**RVSDG (Regionalized Value State Dependence Graph).** A 2019 paper by Reissmann et al. formalises what "keeping the structure" means theoretically. The RVSDG uses *regions* — direct descendants of `{}` — as first-class IR nodes. The central claim, supported empirically, is that most standard optimisation passes are *simpler to implement* on regions than on CFGs, not just more readable. The structural invariants do work that graph algorithms currently do expensively. The information is free at the source level; why pay to reconstruct it?

**QBE.** Quentin Carbonneaux's compiler backend — about 13,000 lines of C, intentionally kept at hobby scale — takes the pragmatic middle path. It uses SSA form with a linear register allocator and several whole-function passes: copy elimination, sparse conditional constant propagation, dead instruction elimination, and registerisation of small stack slots. The last one is the modern equivalent of Turbo C's stack frame elimination: if a local variable never escapes, make it a register, not a stack slot. QBE does this over SSA; Turbo C did it over scopes. The insight is the same.

---

## Why the Compiler Wins Over Time

The 1988 Munich team's output dispels a persistent myth: that handcoded assembly is always faster. It was not — and the reason tells you something important about the five-year horizon.

A human writing 68000 assembly in 1988 optimised for the 68000 as it existed in 1988. They knew its pipeline, its instruction timing, its register file. They could beat the compiler on any given function — for that processor, on that day.

But a compiler is not competing on any given day. It is competing over the lifetime of the codebase.

**What the compiler can do that handcoded assembly cannot:**

- **Global offset and register colouring** — the compiler sees the entire function simultaneously and allocates registers optimally across it. A human writing assembly allocates registers locally, spilling to the stack when they run out. At function scale the compiler wins routinely.
- **Loop unrolling** — tuned to the specific loop body and the target CPU's pipeline depth. The human makes a one-time decision. The compiler recomputes it for every new target.
- **SIMD vectorisation** — a new compiler release adds vectorised code generation for patterns the programmer wrote as scalar loops. The handcoded scalar loop stays scalar forever.
- **Peephole optimisation** — the compiler notices `move.l d0, d1 / move.l d1, d2` and eliminates the intermediate. The human who wrote it by hand never revisits it.

Each of these improves with every compiler release. The handcoded assembly does not. It was correct and optimal for one processor on one day, and it is frozen there.

**The five-year argument is the same as the Moore's Law post.** A human writing 68000 assembly in 1988 could not foresee the 68040's pipeline, the PowerPC's superscalar execution, or ARM's energy profile. The C source compiled for each of those processors by a compiler that understood them. The handcoded assembly required a rewrite — or simply ran slower than compiled C on every subsequent platform.

The compiler that keeps the structure is the compiler that can hand off to the next processor. The handcoded assembly that beats it today is frozen in amber.

---

## The Von Neumann Architecture as Gutenberg Layer

This is also why the von Neumann architecture has survived seventy years of hardware change while alternative execution models have not.

Von Neumann — fetch, decode, execute, shared memory, sequential addressing — is the stable Gutenberg layer beneath every mainstream processor. Intel, AMD, ARM, RISC-V: all von Neumann at the instruction level. The Gutenberg layer has improved continuously (caches, pipelines, superscalar, out-of-order execution, branch prediction) while presenting a stable interface to the semantic layer above — the compiler, the OS, the application.

Two architectures tried a different approach:

**Smalltalk** made message-passing the fundamental operation at the semantic layer and tried to propagate that model down into the execution model. The semantic layer became load-bearing for the Gutenberg layer. Every object lookup was a message send; every message send was a dynamic dispatch. The Gutenberg layer (the CPU) was doing semantic work on every cycle. The result was slow, because semantic overhead does not benefit from pipeline improvements the way sequential instruction execution does.

**Transputers** tried to replace the von Neumann Gutenberg layer entirely with a parallel message-passing hardware architecture — multiple processors on a chip communicating via channels, with parallelism as the fundamental hardware primitive rather than an afterthought. The architecture was technically interesting and genuinely ahead of its time.

Both failed not because they were technically wrong but because they tried to replace the iceberg rather than ride it. The von Neumann iceberg kept getting better — faster, cheaper, more efficient — on a manufacturing and tooling infrastructure built up over decades. Smalltalk and Transputers had to bootstrap their own ecosystem from scratch while the incumbent improved underneath them.

RISC-V is the modern illustration of the correct approach: a clean, open von Neumann Gutenberg layer, fully compatible with the existing toolchain and compiler ecosystem, riding the same foundry improvements as ARM and x86. It does not replace the iceberg. It cleans up the interface while staying on the same iceberg. The semantic layer above it — compilers, operating systems, applications — requires no changes.

The lesson generalises beyond processors. Do not optimise the whole iceberg. Locate the waterline, stay above it, and let the iceberg below improve freely. Smalltalk optimised the iceberg and paid the ecosystem tax. C stayed above the waterline and collected the Moore's Law dividend for fifty years.

---

There is a direct line from this compiler history to the simdjson tape structure, which is worth making explicit.

simdjson parses JSON in two passes. The first pass — vectorised, SIMD-accelerated — marks structural characters: `{`, `}`, `[`, `]`, `:`, `,`, and string/number boundaries. The second pass walks this tape and builds a flat array where each bracket entry contains a jump pointer to its matching close bracket.

The jump pointer is O(1) subtree skipping. You can jump over an entire nested object without reading its contents. This is exactly what `{}` gives a compiler: the ability to skip a scope without analysing its contents, because the structure is explicit and the boundary is a token, not a measurement.

The [tape-tokenizer](https://github.com/rinie/tape-tokenizer) project applies the same principle to source code: a flat Uint32Array tape with structural tokens at ASCII bracket values, an intern pool for O(1) identifier comparison, and comment trivia stored out-of-band in a side array rather than interleaved with the structural tokens. The signal — identifiers, keywords, brackets — stays on the tape. The noise — whitespace, comments — goes to the side array where you can retrieve it when you need it and ignore it when you don't.

This is the Gutenberg/Semantic boundary applied to parsing. The bracket tape is Gutenberg: physical positions, fixed-width entries, cache-friendly. The intern pool is Semantic: names, meanings, O(1) equality. The trivia side array acknowledges that comments are neither — they are documentation noise that belongs in a separate channel.

---

## Why This Matters Beyond Compilers

The SNR principle that Czymontek et al. applied to a 68000 compiler in 1988, and that RVSDG formalises today, is not specific to compilers. It is the same principle as the blog post you are reading being structured with headings rather than typeset as a continuous flow of paragraphs.

**Structure is a Def.** The `{}` that opens a scope, the heading that opens a section, the region that opens an RVSDG node — these are all Defs. They push the structural information to the reader (human or compiler pass) at the point where the structure begins.

**Flat representation is Use-Pull.** The basic block graph, the undifferentiated paragraph flow, the newspaper without headlines — these require the reader to pull the structure back out by analysis. The information was there at the Def site. It was destroyed in transit. The reader pays the reconstruction cost at every Use.

At small scale — a 50-line function, a 3-paragraph article — Use-Pull is tractable. The reconstruction is cheap and the error rate is low. At 10x scale — a 500-function codebase, a 30-post blog series, a compiler processing a million-line program — the reconstruction cost multiplies with every reader and every pass. The flat representation that looked clean at small scale has become the newspaper without headlines: technically complete, practically unnavigable.

The 1988 Munich compiler team never had the luxury of small scale. They were working on a machine with 512KB of RAM, in a language where every cycle counted, targeting programmers who would notice immediately if the generated code was worse than what they could write by hand. So they kept the structure. They paid the 10% design overhead of a scoped IR and collected the dividend in every function they compiled.

Thirty-five years later, the compiler research community is writing papers to prove they were right.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). The companion post [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) covers the same waterline idea at the infrastructure level rather than the compiler level.*
