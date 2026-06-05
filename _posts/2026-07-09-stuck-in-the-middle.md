---
layout: post
title: "Stuck in the Middle: The JVM, CIL, and the Billy That Had to Read the Book"
date: 2026-07-09
tags: [gutenberg-semantic, jvm, wasm, objects, registers, intermediate-representation]
level: technical
description: "The JVM and CIL tried to be the universal Gutenberg layer but baked the Semantic layer into the substrate. Stack-based instead of register-based. Objects instead of values. A Billy bookcase that accepted CD cases, refused Blu-ray, and had to read every book before it would hold it. WASM got the layers right."
---

The CD case is 142mm × 125mm. The DVD case is 190mm × 135mm. The Blu-ray case is 171mm × 135mm.

The disc is 120mm in all three.

A Billy bookcase designed for CD cases cannot hold DVD cases cannot hold Blu-ray cases — even though the actual disc, the actual content, the actual Gutenberg artifact is identical in size across all three. The Gutenberg unit (the disc) stayed the same. The Semantic packaging (the case) changed with each generation. The shelf accepted the case as the unit. The disc was above the waterline. The case was not.

The JVM is the CD-case Billy. The disc is the bytecode. The case is the object model, the stack frame, the GC metadata, the class header. Each generation of the runtime accepts its own case format. Blu-ray was refused.

And unlike the real Billy — the JVM had to read the book before it would hold it.

---

## Stuck in the Middle

The JVM and CIL (.NET's Common Intermediate Language) tried to be the universal Gutenberg layer. A stable intermediate representation that any language could compile to, any platform could execute, any tool could analyse. The promise: write once, run anywhere. The waterline drawn at the bytecode.

The problem: they baked the Semantic layer into the Gutenberg layer.

**Too high for hardware.** The JVM needs a JIT to run on a real CPU. Every execution requires translating stack operations to register operations — work a native compiler does once at compile time, paid again at every startup. The bytecode is not the hardware's native language. It is an abstraction above the hardware that pretends to be below the application.

**Too low for language.** The JVM erases the type information the language had. A Java `List<String>` becomes a `List` at the bytecode level — type erasure. The generic type parameter that the compiler knew about is gone by the time the JIT sees it. Semantic information lost at the waterline, not preserved below it.

**Carrying Semantic information in the Gutenberg layer.** Every JVM object has a header — typically 8-16 bytes of metadata: class pointer, lock word, GC age bits. A Java `Integer` containing the value `42` takes 16 bytes on the heap. An int in a register takes 4 bytes. The object header is Semantic information (what type is this, is it locked, how old is it for GC purposes) fused to the Gutenberg carrier (the bits that represent 42).

Stuck in the middle. Not the hardware. Not the language. The case on the shelf.

---

## Stack-Based Instead of Register-Based

The JVM is a stack machine. Operations push values onto the stack, pop them off, produce results. No explicit registers. The architecture is a stack — a Semantic abstraction of computation that does not correspond to how any real CPU works.

Real CPUs have registers — a fixed number of fast storage locations directly in the processor. The compiler's job is to map programmer variables (Semantic) to registers (Gutenberg) efficiently. The register allocator is the waterline. Good register allocation is the difference between fast code and slow code.

The JVM hides the register allocation inside the JIT. Every time the JVM executes, it translates stack operations to register operations — doing work that a native compiler does once at compile time, paid at every startup. The JIT gets good at this. It is always paying the translation cost. The stack is the wrong Gutenberg unit for the hardware.

**Infinite registers instead:**

Modern compiler intermediate representations — LLVM IR, WebAssembly, SSA form — use an infinite register model. Each value is assigned once to a virtual register. The register allocator maps virtual registers to physical registers at the end. The intermediate representation is clean, flat, positional. The breadth-first tape. Each value is a separate tree. None of the roots are entangled.

The JVM stack is depth-first execution dressed as a Gutenberg layer. LLVM IR is the flat tape that shows where the pace changes.

---

## Objects Instead of Values

The JVM object model is the Universal Tree Fallacy baked into the runtime. Every value is an object. Every object has a header. The integer IS an object IS a heap allocation IS a GC-traced reference.

This is the is-a collapse at the runtime level. There is no seam between the value (Semantic) and the carrier (Gutenberg). The GC needs to know which allocations are objects so it can trace references. The type system needs the class pointer so it can dispatch methods. The Semantic information travels with every Gutenberg byte.

The Billy had to read the book before it would hold it. Every allocation checked against the class hierarchy. Every method call dispatched through the vtable. Every GC cycle tracing the table of contents to find which books point to which other books.

The Billy that reads the book cannot scale. Every book requires understanding. Every shelf requires knowledge of every genre. The Billy that holds things of the right size scales to any library, any content, any format not yet invented.

**jemalloc does not read the book:**

jemalloc sees bytes. It does not know whether the 64-byte allocation is a string, a struct, a node, or a tax record. It knows: size class, arena, age. The allocator reasons entirely in terms of size and age — no semantic knowledge required. The page never shrinks. The last page may be smaller. The allocator returns at least what was requested. The content above the waterline is none of its business.

The JVM allocator must know the object type because the GC must trace the references. The allocator and the GC are fused. The bookcase and the librarian are the same system. Neither can be replaced without replacing both.

---

## The Disc Is Always 120mm

The CD → DVD → Blu-ray → 4K UHD evolution is the correct Gutenberg story. Same 120mm disc. Ever-denser encoding. Same laser principle extended by wavelength. The Gutenberg unit evolved upward in capacity without changing size. The page grew. The byte stayed 8 bits. The disc stayed 120mm.

The cases were marketing artifacts. They could have stayed the same throughout. The JVM made the same mistake as the disc cases — it changed the packaging with each generation and called the packaging the unit.

JVM bytecode v49 (Java 5), v50 (Java 6), v51 (Java 7) — each version a slightly different case. The disc (the computation) did not change. The case (the bytecode version, the object model, the GC interface) changed enough that each version required its own Billy.

**WASM as the disc without the case:**

WebAssembly is the disc handed directly to the drive. No case. The content is typed — i32, i64, f32, f64, v128 — but the types are Gutenberg types, not Semantic packaging. Not Integer, not String, not Object. Linear memory that the application manages directly. No object headers. No GC built into the runtime. No class metadata stapled to every value.

The drive reads the disc. The application above the waterline decides what the content means. The Billy holds every disc because the Billy was designed for discs, not for cases.

WASM is also register-based — values are locals and stack slots explicitly managed, not an implicit operand stack. The JIT maps them to physical registers without a translation layer. The Gutenberg unit maps cleanly to the hardware. The waterline is in the right place.

---

## What the JVM Got Right

The JVM was not wrong. It was early. It arrived before the vocabulary existed to name what it was doing wrong.

Write once, run anywhere was a real problem in 1995. Every platform had different native APIs. Every C program needed platform-specific compilation. The JVM solved real fragmentation with a real solution — one bytecode, many platforms. The waterline was approximately correct. The seam existed. It just carried too much semantic baggage across it.

The JIT improved dramatically — HotSpot, GraalVM, the JVM today is genuinely fast for many workloads. Escape analysis lets the JVM sometimes avoid heap allocation for short-lived objects, treating them as values rather than objects. Value types are arriving in Project Valhalla. The JVM is slowly moving the semantic baggage above the waterline where it belongs.

But it is working against gravity. The is-a collapse is baked into thirty years of Java code, thirty years of libraries, thirty years of frameworks built on the assumption that everything is an object, everything is on the heap, everything has a class pointer.

The CD-case Billy trying to accommodate Blu-ray by making the shelves adjustable. Possible. Expensive. The books are still the wrong shape.

---

## WASM Is Not the Destination

WASM is the correct Gutenberg layer for the current era. But it is not the destination — it is the next turn.

The page size grew from 512 bytes to 4K to 2MB huge pages. WASM's linear memory will grow. WASM's type system will extend. WASM threads, WASM GC, WASM components — each one a seed planted alongside the existing WASM, not a replacement.

The invariant holds: the byte is 8 bits. The disc is 120mm. The Billy holds things of the right size. The content is above the waterline.

The JVM and CIL are stuck in the middle — not hardware, not language, a case design from 1995 that the world is slowly building around rather than through. The disc is the same size it always was. The Billy does not have to read the book.

Plant new trees. Keep the diff clean. The byte is still 8 bits.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Frameworks Move the Seam](https://rinie.github.io/2026/07/06/frameworks-move-the-seam/) on infra changes being very expensive, [Buffer Overflow Is Printing Outside the Page](https://rinie.github.io/2026/06/29/buffer-overflow-is-printing-outside-the-page/) on Gutenberg versus Semantic errors, and [The Watershed](https://rinie.github.io/2026/07/02/the-watershed/) on Android choosing the kernel and leaving the object model behind.*
