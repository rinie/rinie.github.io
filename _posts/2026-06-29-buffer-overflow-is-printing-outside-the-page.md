---
layout: post
title: "Buffer Overflow Is Printing Outside the Page"
date: 2026-06-29
tags: [gutenberg-semantic, rust, buffer-overflow, errors, utf-8, type-system]
level: technical
description: "A buffer overflow is a Gutenberg violation — you wrote past the physical extent of the medium. A type error is a Semantic error — the text is on the page but doesn't parse as meaningful. Rust is the first mainstream language that keeps the two channels separate by design."
---

There is a question that looks like it is about memory safety but is actually about layers.

A buffer overflow: you wrote past the allocated address space. The bytes landed where there was no paper. This has nothing to do with what the bytes *mean* — it is purely that you addressed a location outside the allocated block. The failure is positional, physical, Gutenberg-layer. You printed outside the page.

A type error: you treated a string as a number, called a method that does not exist on this type. The text is all on the page, well-formed positionally, but it does not parse as meaningful. The failure is at the Semantic layer — the words are in the right place but they do not mean what you asked them to mean.

Same word, "error." Different layer. Different cause. Different correct response.

---

## The Two Channels

The [Gutenberg/Semantic model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) gives these two failures their correct names and their correct fixes.

A **Gutenberg error** is a physical violation — the page tore. Buffer overflow, stack corruption, misaligned memory access. The substrate failed. The correct response is the one for physical emergencies: stop, do not proceed, the building is structurally compromised. In software: crash, panic, abort. Proceeding with a corrupted substrate produces undefined behaviour — not a wrong answer but a meaningless one.

A **Semantic error** is a logical failure — the meaning did not parse. Division by zero, a missing key, a string that is not a valid number. The substrate is intact. The page is there. The bytes are there. The meaning just does not work out. The correct response is the one Excel uses for `###`: display the failure as a value, not an emergency. Put it in the return position where the caller can see it and decide what to do with it.

The failure mode of most languages is conflating these two channels. An exception is a control-flow escape — a semantic error (this key is missing) that hijacks the call stack and unwinds it like the building is on fire. Stack unwinding is a Gutenberg-layer mechanism (positional, frame-by-frame) deployed to signal a meaning-level problem. It travels a hidden channel you did not write and cannot see in the type signature. It dresses a semantic error as a Gutenberg emergency.

---

## The `reinterpret_cast` Edge Case

There is a third case worth naming — the type cast that crosses layers.

A *semantic* type error stays on the page. You named the wrong type; the compiler catches it; nothing moves in memory. Punctuation error, no physical consequence.

A *reinterpret cast* — C's `reinterpret_cast`, reading the same bytes through a different struct layout — is sneakier. The cast itself is semantic (you renamed what the bytes mean), but it can induce a Gutenberg violation downstream. If the new type is larger or differently aligned, you are now reading or writing past where the original bytes physically sat. A punctuation error that tears through the page.

This is the precise thing name-trust does not guarantee byte-trust. A purely semantic act (renaming the type) is trusted to also be physically safe, and C lets that trust go unchecked. The semantic layer impersonates the Gutenberg layer. The layers couple. The error from one layer produces damage in the other.

---

## Rust Does the Envelope/Letter Separation

Rust is the first mainstream language that keeps the two channels separate by design — not as a convention, not as a style guide, but as a structural property of the type system.

`Result<T, E>` refuses the exception escape. The error is a *value*, in the return position, on the page. It does not travel a hidden path. It sits in the type signature where you read it. `?` propagates it explicitly rather than by ambient unwinding. That is the `###`-in-the-cell discipline applied to control flow: the failure is data that says "this did not mean anything," not an emergency that tears through the frames.

`panic!` still exists and still unwinds — but it is reserved for the genuine Gutenberg emergency: index past the end, the page really did tear. Two channels, two failure modes, each on the right layer.

On strings: `String` / `&str` guarantee valid UTF-8 — a Semantic-layer invariant ("this is text, it means characters"). `Vec<u8>` / `&[u8]` are pure Gutenberg — bytes, positions, no meaning claimed. `OsStr` is the honest middle: "bytes the OS calls a string but won't promise are UTF-8." The layers are explicit rather than collapsed into one `String` type that silently does both.

The legitimate gripe is that the ergonomic default — string literals, most APIs, `format!` — is the Semantic type. For systems work at the Gutenberg layer, you swim upstream. Not too strict — mis-defaulted for that domain. The `bstr` crate exists precisely to flip the default for people who live at the Gutenberg layer.

---

## The Correct Error Model

The two-layer model produces the correct error taxonomy:

**Gutenberg errors** — physical violations, non-recoverable, abort:
- Buffer overflow — wrote past the page
- Stack corruption — tore the frame
- Misaligned access — addressed the wrong medium
- Segfault — page does not exist

**Semantic errors** — logical failures, recoverable, return as values:
- Missing key — the map does not contain this meaning
- Parse failure — the bytes do not decode as this type
- Division by zero — the operation has no meaningful result
- Type mismatch — the semantic label is wrong

**The conflation** — semantic errors dressed as Gutenberg emergencies:
- Exceptions for missing keys
- Null pointer exceptions for "no value here"
- `NullPointerException` for a meaning-level absence
- Stack unwinding for a value-level failure

Rust's `Result<T, E>` is not strict. It is correctly layered. The error is on the right layer, in the right channel, visible in the right place. The panic is reserved for when the page actually tears.

Keep the errors on the layer where they live. The building is not on fire. The key is just missing.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Exceptions, Result/Option, and HTTP: Error Handling as Def-Use](https://rinie.github.io/2026/05/23/exceptions-result-option-http/) on errors as values, [The Compiler That Knew Where It Was](https://rinie.github.io/2026/05/31/compiler-snr-evolution/) on Rust's SSA and type model, and [Hiding the Waterline Makes You Drown Without Knowing Why](https://rinie.github.io/2026/06/08/hiding-the-waterline/) on what happens when layers impersonate each other.*
