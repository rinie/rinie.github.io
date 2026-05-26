---
layout: post
title: "Worse Is Better Because the Gap Is Where Evolution Happens"
date: 2026-06-01
---

In 1991, Richard Gabriel wrote an essay that has haunted software engineers ever since. He called it "Worse is Better" and its thesis was uncomfortable: the Unix/C approach — simpler, less correct, more permissive, aesthetically inferior — had beaten the Lisp approach — more expressive, more correct, more powerful — and would continue to beat it. He framed this as a kind of civilisational tragedy. The worse design won. What does that say about us?

He was right about the observation. He was wrong about the explanation.

---

## What Gabriel Saw

Gabriel contrasted two design philosophies. The "MIT/Stanford approach" — which he also called "the right thing" — prioritised correctness, consistency, and completeness of the interface over simplicity of implementation. If the interface had to be complex to be correct, so be it. The implementation could absorb the complexity.

The "New Jersey approach" — Unix/C — prioritised simplicity of implementation above everything else, including interface correctness. If the interface had to be slightly wrong or incomplete to keep the implementation simple, that was acceptable. Worse was better.

The New Jersey approach won. Unix spread to every hardware platform. C became the lingua franca of systems programming. Lisp retreated to academia and specialised domains. Gabriel was mourning this outcome and trying to explain it — reaching for cultural and sociological explanations about hacker aesthetics and viral adoption.

What he was actually describing, without the vocabulary to name it, was the Gutenberg/Semantic boundary.

---

## The Boundary Gabriel Did Not Name

Unix/C kept a clean separation between the Gutenberg layer — the kernel, the hardware, the C runtime — and the semantic layer above it. The "worse" design was worse at the semantic layer: more undefined behaviour, more implicit conventions, more ways to shoot yourself in the foot. But it was better at the Gutenberg layer: thin, portable, easy to reimplement on new hardware, easy to recompile for a new architecture.

When a new processor arrived — faster, cheaper, better — Unix just recompiled. The semantic layer above the boundary was unchanged. The Gutenberg layer below it was replaced. The improvement was free.

Lisp and the Lisp machines collapsed the boundary. The semantic model — dynamic typing, garbage collection, message passing, the entire Lisp object system — was not an abstraction over the hardware. It was the hardware. Symbolics and LMI built processors designed to execute Lisp semantics directly. The fetch-decode-execute cycle was replaced with a specialised microarchitecture tuned for Lisp's specific computational model.

This was technically impressive. It was also a dead end — not because the engineering was bad, but because the boundary was gone. When faster commodity hardware arrived, the Lisp machine could not benefit. The commodity hardware was improving the wrong layer. The Lisp machine's iceberg was the wrong shape.

---

## The Lisp Machine at Full Scale

The Lisp machine story is the Transputer and Smalltalk story told with venture capital and a decade of serious engineering. Symbolics, LMI, Xerox PARC — all building hardware optimised for a specific semantic model. Each machine was genuinely impressive. Symbolics workstations in the mid-1980s ran circles around contemporary Unix workstations on Lisp-heavy workloads — AI research, symbolic mathematics, expert systems.

Then commodity hardware improved. Moore's Law kept compounding on the von Neumann architecture. By the early 1990s, a Sun workstation running a good Common Lisp implementation was competitive with a Symbolics machine — and the Sun workstation also ran C, Fortran, and everything else. Symbolics filed for bankruptcy in 1996.

The semantic gap that the Lisp machine eliminated was load-bearing. Its absence meant the Gutenberg layer could not evolve independently. Every hardware improvement required rethinking the microarchitecture to maintain Lisp performance. Every compiler improvement for the commodity platform was free. Symbolics had to do in hardware what GNU got for free from Intel.

---

## RISC Was Right. x86 Won Anyway.

The RISC versus CISC debate of the 1980s and 1990s is the same story with a twist.

RISC was right at the semantic layer. A regular instruction set, explicit load/store, no microcode, fixed-width instructions — all of these are cleaner, more regular, more amenable to compiler optimisation. RISC processors were easier to pipeline, easier to superscale, easier to reason about. Every computer architecture textbook since Patterson and Hennessy has taught RISC principles as the correct approach.

x86 is a CISC mess. Variable-length instructions. Implicit side effects. Microcode. An instruction set that accumulated thirty years of backward-compatible cruft from the 8086 through the Pentium. Semantically inferior by any objective measure.

x86 won anyway — and the reason is the Gutenberg/Semantic boundary.

Intel kept the x86 instruction set architecture (the Gutenberg interface) stable across decades while radically improving the microarchitecture underneath it. By the time the Pentium Pro arrived in 1995, x86 chips were internally RISC processors with a CISC translation layer at the front door. The complex x86 instructions were decoded into simple micro-operations (µops) — RISC instructions — which the superscalar out-of-order execution engine processed. The semantic gap between the x86 ISA and the actual hardware execution was enormous. That gap was load-bearing.

Every application, every operating system, every compiler that targeted x86 kept working. The Gutenberg layer below them improved by a factor of hundreds. The software above never noticed. The RISC workstations — MIPS, SPARC, Alpha, PA-RISC — each had a better Gutenberg interface but a smaller ecosystem, and the ecosystem improvements compounded slower.

Apple Silicon is the current chapter of the same story. ARM is a cleaner ISA than x86. The M-series chips are dramatically more efficient. Rosetta 2 is the x86-to-ARM translation layer — the semantic gap preserved in software rather than hardware, so the existing software ecosystem could move to the new iceberg without a flag day. The gap did not disappear. It moved from the CPU's front-end decoder to Apple's binary translation layer. The principle is the same: preserve the boundary, let the Gutenberg layer evolve.

---

## Evolution Is Smarter Than You

This is Chesterton's Fence applied to the entire history of computing.

The von Neumann architecture was not designed to be optimal. John von Neumann's 1945 report was a pragmatic compromise — fetch instructions from memory, execute them sequentially, store results. It was not derived from first principles as the correct model of computation. Turing machines, lambda calculus, and dataflow models all have stronger theoretical foundations.

But von Neumann survived because it maintained a clean interface that allowed independent evolution on both sides. Compilers above it. Manufacturing below it. Neither side needed to know what the other was doing. Intel could redesign the pipeline completely — in-order to out-of-order, single-core to multi-core, monolithic to chiplet — without touching the ISA. Compiler writers could improve register allocation, loop unrolling, and vectorisation without touching the silicon.

Every attempt to "fix" von Neumann by collapsing the layers has foundered on the same problem: the evolution of the Gutenberg layer is not under any individual designer's control. It is driven by fabrication physics, manufacturing economics, tooling ecosystems, and accumulated engineering knowledge spread across thousands of companies and research institutions. No single design team can predict or control where that evolution goes. The architecture that survives is the one that stays out of its way.

Lisp machines tried to make the hardware smarter about Lisp. The hardware got smarter about everything else instead.
Transputers tried to make parallelism the fundamental hardware primitive. The industry parallelised von Neumann instead, with caches, pipelines, and eventually multi-core.
Dataflow machines tried to eliminate the sequential execution bottleneck. Out-of-order execution eliminated it within von Neumann, invisibly, beneath the same ISA.

In each case, the "correct" architecture predicted a specific direction for hardware evolution and optimised for it. The von Neumann architecture predicted nothing and accommodated everything.

This is what "evolution is smarter than you" means in practice. Not that design is useless — the clean Gutenberg/Semantic boundary is itself a design decision, and a crucial one. But the content of the Gutenberg layer's evolution — which specific improvements arrive, in which order, on which timeline — is not predictable from first principles. The correct response is not to predict it and optimise for it. The correct response is to stay above the waterline and collect whatever improvements arrive.

---

## The Unified Principle

Gabriel was right that worse beat better. His framing was wrong about why.

The New Jersey approach won not because Unix hackers had lower standards or because viral adoption favoured simple systems. It won because it preserved the semantic gap. The gap is not a flaw to be eliminated by a sufficiently clever design. It is the space where forty years of Moore's Law happened.

Every design that collapsed the gap — Lisp machines, Smalltalk, Transputers, CORBA, gRPC, Java applets — lost to a design that preserved it. Not because the collapsed design was wrong at the semantic layer. Often it was more correct, more expressive, more powerful. But correctness at the semantic layer is a local optimisation. Preserving the Gutenberg/Semantic boundary is a global optimisation over the entire future evolution of the platform.

You need the semantic gap so the Gutenberg layer may evolve. The gap is load-bearing. Before removing it — before collapsing your abstraction layers in the name of correctness or performance or elegance — ask what the gap is separating, and what would happen if the Gutenberg layer improved in a direction you did not predict.

It will. It always does. And evolution is smarter than you.

---

## C++ Versus C and Rust: The Kernel as the Proof

The Linux kernel is the longest-running practical demonstration of this principle, and Linus Torvalds has been its most blunt advocate.

**C in the kernel** is a thin semantic layer over the Gutenberg hardware. The C abstract machine maps almost directly to the von Neumann ISA. Pointers are addresses. Structs are memory layouts. Function calls are stack frames. There is almost no distance between what C says and what the hardware does. The semantic gap is minimal — but it is present, and it is in exactly the right place: between the C source and the specific machine code the compiler emits. The compiler fills the gap. The programmer stays above it.

Linux is built on replaceable components connected by stable C interfaces. A filesystem driver, a network driver, a scheduler — each is a module that implements a defined interface. The interface is a C struct of function pointers. Swap the implementation, keep the interface, the rest of the kernel does not care. This is the Gutenberg/Semantic boundary applied to OS architecture: the interface is semantic (what this component does), the implementation is Gutenberg (how it does it on this hardware). The kernel evolves by replacing implementations without touching interfaces.

**C++** takes a different approach. C++ assumes you compile the iceberg. Templates expand at compile time into hardware-specific code. Virtual dispatch tables are embedded in the object layout. The C++ object model bleeds into the ABI — change a class, recompile everything that uses it. The RTTI metadata, the exception tables, the vtable layout: all of these are semantic decisions baked into the Gutenberg artifact (the binary). The gap between the C++ semantic model and the machine code is not clean. It is negotiated, version by version, compiler by compiler, ABI by ABI.

Linus has been explicit about why C++ is not welcome in the kernel. His objections are not aesthetic. They are structural: C++ makes it too easy to write code that looks like it respects the interface boundary but actually depends on implementation details. The object model encourages inheritance hierarchies that couple callers to the internal structure of their dependencies. Templates encourage inlining that destroys the modularity the interface was supposed to provide. The C++ iceberg is harder to replace because too much of the semantic layer has been compiled into it.

**Rust** is a different argument entirely. Rust also compiles to efficient machine code. It also has zero-cost abstractions. In those senses it resembles C++. But Rust's ownership model is a semantic constraint that the compiler enforces — it lives above the boundary, not below it. The borrow checker does not generate different machine code for different hardware; it generates a proof at compile time that the semantic contract is satisfied, then emits the same lean machine code that C would emit. The Gutenberg layer is as thin as C. The semantic layer is safer than C++.

Linus's acceptance of Rust in the kernel — cautious, conditional, with the explicit requirement that Rust code must not break existing C interfaces — reflects exactly this distinction. Rust is acceptable because it respects the boundary. It adds semantic safety above the waterline without thickening the Gutenberg layer below it. C++ is not acceptable because it compiles the iceberg: its abstractions leak into the binary in ways that make the Gutenberg layer harder to evolve.

The kernel's position on C++ versus Rust is not a language war. It is a boundary maintenance policy. The kernel is the Gutenberg layer for everything above it. Anything that makes the kernel harder to replace, harder to port, or harder to evolve independently of its callers is a threat to the boundary that has kept Linux running on everything from wristwatches to supercomputers for thirty years.

C keeps the gap. Rust keeps the gap with better semantics above it. C++ fills the gap with its object model and calls it a feature.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related posts: [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) on portability as a basket option, and [The Compiler That Knew Where It Was](https://rinie.github.io/2026/05/31/compiler-snr-evolution/) on keeping structure through compilation.*
