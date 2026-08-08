---
layout: post
title: "The Migration Was Good. The Destination Is Still Debatable."
date: 2026-08-20
tags: [gutenberg-semantic, python, utf-8, unicode, pep393, log-cabin, resolver]
level: technical
description: "Python 2 to 3 had a bridge Perl 6 never built, and that comparison still holds. But the actual breaking change was the str/bytes type split, not the internal encoding — UTF-8 internally would have broken exactly the same amount of code. Twelve years later, CPython still hasn't switched, though its own core developers have an open issue proposing exactly that. They had a problem. They shipped something. The problem is still there."
---

[A recent post here](https://rinie.github.io/2026/08/16/perl6-python3-walled-garden/) compared Perl 6's fifteen-year, ship-less rewrite against Python's 2-to-3 transition, and credited Python for building a bridge Perl never did — years of parallel support, conversion tooling, a path anyone could walk at their own pace. That comparison holds up. It was conflating two separate claims, though, and only one of them survives scrutiny: *the migration process was well executed* is not the same claim as *the destination it migrated to was well designed*. [UTF-8 Everywhere](https://utf8everywhere.org/), a manifesto arguing UTF-8 should be the default internal string representation everywhere, makes a specific, documented case that the second claim doesn't hold.

---

## Three Encodings, Chosen at Runtime

CPython 3.3 didn't move to UTF-8 internally. It introduced what's called a flexible string representation — the interpreter picks one of three internal encodings for a given string, Latin-1, UCS-2, or UCS-4, based on the widest character actually present in it. A string of plain ASCII text gets the narrowest representation. Add a single character outside that range, and the entire string can be silently re-encoded to a wider one to accommodate it.

The manifesto's stated reason this was the wrong call: *"they should have done less."* The design optimises for one specific operation — constant-time indexing by Unicode code point — that the document argues isn't actually the operation that matters most. Code points don't correspond to what a user perceives as a single character; grapheme clusters do, and Python has no built-in support for those. Meanwhile the operation that dominates real-world text handling — treating strings as UTF-8 byte sequences for storage, transmission, and interchange with HTTP, JSON, and the rest of the web — gets no particular advantage from this design at all.

---

## The Log Cabin, at String-Representation Scale

This is a smaller-scale instance of [the exact failure Wirth's Law traced through the JVM and .NET](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/): build a comprehensive internal representation from scratch, optimised for an operation the language designers judged important, rather than resolving against the representation the surrounding ecosystem had already converged on. Rust, Go, and Swift all settled on UTF-8 as their internal string representation. The web, HTTP, JSON — the actual wire format software spends most of its life talking to — is overwhelmingly UTF-8. Python's flexible representation solves a problem (fast code-point indexing) that turns out to matter less than the problem it doesn't solve (staying in the same encoding as everything the string will eventually be serialised to or read from).

The leaky-abstraction cost is the tell. A string that silently changes its internal representation when a single new character gets appended is exactly the kind of implementation detail [Joel Spolsky's user-model argument warns against](https://rinie.github.io/2026/08/18/program-model-is-the-def-model/) — invisible to the programmer using the string, until a performance cliff or a memory-usage surprise reveals that the "just a string" abstraction was never as flat as it looked.

---

## Two Claims, Correctly Separated

The migration mechanics still deserve the credit given in the earlier post — a decade of parallel support, `2to3`, libraries declaring dual compatibility, nothing forced to happen all at once. That's real, and it's still the reason Python's transition was survivable where Perl's wasn't. But "the bridge was well built" says nothing about whether the destination on the other side of it was the right one to build a bridge to. UTF-8 Everywhere's critique is specifically about the destination, and it's a fair one: a comprehensive, custom internal representation, optimised for an operation most code doesn't actually need, instead of resolving against the encoding the rest of the ecosystem had already settled on. That's not a flaw in how Python got from 2 to 3. It's a flaw in what Python 3 turned out to be.

## The Break and the Encoding Were Two Different Decisions

There's a sharper version of this critique worth making explicit, because it changes what the mistake actually was. The breaking change that caused most of the real migration pain wasn't the internal string encoding at all — it was the type model. Python 2's `str` and `unicode` could be implicitly coerced into each other, which worked fine until it hit non-ASCII text and then failed, often in production, with a `UnicodeDecodeError` nobody had budgeted time to chase down. Python 3's actual fix was separating `str` and `bytes` into genuinely distinct types with no implicit coercion between them — mixing them now raises an error immediately, at the point of the mistake, instead of failing silently somewhere downstream. That's what broke `print`, broke file I/O assumptions, broke string formatting across a huge amount of existing code. It was closer to fixing a real defect than changing a convention — the same distinction [Joel's essay draws](https://rinie.github.io/2026/08/18/program-model-is-the-def-model/) between a user model that's merely unfamiliar and one that's factually wrong.

The internal representation — UCS-2, UCS-4, PEP 393's flexible scheme, or UTF-8 — sits on a completely separate axis, invisible to almost all application code. Choosing UTF-8 internally instead of the flexible representation would not have required a different `str`/`bytes` split, would not have changed I/O behaviour, would not have saved `print` its parentheses. The migration would have broken exactly the same amount of existing code either way, because the type-model fix was the load-bearing change and the internal encoding was not.

Which means the honest framing isn't "Python chose a worse destination instead of a better one at the same cost as staying put." It's narrower and sharper than that: Python paid the full migration cost regardless of which encoding it picked, and got the worse destination anyway, when the better one was sitting right there for the same price. The break wasn't the mistake. The specific representation chosen once the break was already unavoidable was.

---

## Twelve Years Later, Still Unresolved

Here's the part that makes this more than an outsider's grievance: CPython still hasn't moved to UTF-8 internally. PEP 393's flexible representation, adopted in 2013, remains what every current version of the language actually does. But there's an open issue in [faster-cpython/ideas](https://github.com/faster-cpython/ideas/issues/684) — the official CPython performance initiative, staffed by the language's own core developers — titled, plainly, *"Use UTF-8 internally for strings."* Not a rejected proposal. An open one, sitting on the table, more than a decade after the original decision, raised from inside the same community that made it.

That's the honest final shape of this story: Python had a real problem — internal Unicode representation that genuinely needed fixing in the 2.x era — and the fix it shipped, PEP 393, solved the immediate memory and correctness issues well enough to ship, while leaving the deeper representation question exactly where the critics said it was. Not fixed. Not defended into obsolescence, either — still visibly, actively open, twelve years on, in the project's own issue tracker. They had a problem. They shipped something. The problem, in the specific form the manifesto named, is still there.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Perl 6 Promised Better. Python 3 Just Shipped.](https://rinie.github.io/2026/08/16/perl6-python3-walled-garden/) on the migration this post narrows the claim about, and [Wirth's Law Only Holds If You Recompile the Whole Iceberg](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/) on the log cabin this string design repeats in miniature.*

Sources: [UTF-8 Everywhere](https://utf8everywhere.org/), Pavel Radzivilovsky, Yakov Galka, and Slava Novgorodov; ["Use UTF-8 internally for strings"](https://github.com/faster-cpython/ideas/issues/684), faster-cpython/ideas, GitHub.
