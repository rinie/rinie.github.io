---
layout: post
title: "The Intuition Had a Shape"
date: 2026-06-26
tags: [intuition, breadth-first, ssa, compilers, def-use, tape, gutenberg]
level: deep-dive
description: "A dislike I carried for thirty years turned out not to be a quirk but a design principle. Breadth-first, flat, weighted toward the reader — from TurboC to a flat token tape, the same stance the whole way down."
---

There is a particular feeling every experienced programmer knows and almost no one writes down: disliking something before you can say why. Not a reasoned objection — a flinch. A tool, a notation, a methodology that everyone around you accepts, and something in you quietly refuses it. You can't defend the refusal, so you keep it to yourself, and for years it sits there as a private prejudice you half-suspect is just you being difficult.

I want to make a case for that flinch. Because I've been carrying one for about thirty years, and it recently resolved — all at once — into something I can finally state plainly. It was never a prejudice. It had a shape.

## The flinch

Mine was a cluster of dislikes that never seemed connected. I disliked hand-written assembly when a compiler was available. I disliked the abstract syntax tree being treated as the *real* program. I disliked stack-based parsers and their error messages. I disliked automatic semicolon insertion and I disliked IntelliSense finishing my thoughts. I disliked the textbook story that a book *is* chapters *is* paragraphs *is* sentences, all the way down.

Five unrelated grievances, or so I thought. Defending any one of them in isolation sounds like crankiness. Together they turn out to be a single idea, and the idea is about *direction*.

## Breadth-first is how I actually think

Niklaus Wirth gave us stepwise refinement, and the textbooks quietly turned it depth-first: take one step, refine it completely, then move to the next. That is not how I work, and I no longer think it's how the method was meant.

I work in strata. Outlines first — the whole width of the thing at the shallowest depth. Then loops and blocks — the skeleton, again across the whole width. Then I fill. I lay down every section heading before I write any section. I sketch every function shell before I implement any function body. I am completing a *level* across its full breadth before I descend, not finishing one branch before starting the next.

This is breadth-first refinement, and once you name it, the five dislikes line up like iron filings.

## TurboC, and why the compiler wins

Start with assembly. The reason I stopped hand-writing it on the 68000 and the PDP-11 and reached for Turbo C instead was not laziness. It was that the compiler keeps track of the live variables better than I can. At any point in a program there is a *set* of values currently in play — that's a breadth fact, the full width of "what matters right now." Hand-assembly forces you depth-first into the register file, spilling and reloading one value at a time, and the global picture slips away from you. The compiler holds the whole set. It is a better breadth-first bookkeeper than a human, and that — not instruction count — is why its output beats mine.

So the preference for the high-level language over assembly was already the breadth-first stance. I just didn't have the words.

## SSA keeps the breadth; the AST eats it

Here is the part that surprised me, because it's where compiler theory itself takes a side.

Static Single Assignment form — the representation modern compilers use internally — is *easy* to understand. Every value is defined exactly once and named by where it's defined. The structure is flat and positional; you query the relationships over it rather than climbing into them. SSA keeps **define** and **use** as first-class facts between positions. It stays wide.

The abstract syntax tree and the directed graph do the opposite. They re-nest the program into a shape, and in folding it back into a tree they quietly dissolve something I care about: the special standing of the *top level*. In an AST, a top-level function and a deeply buried sub-expression are both just nodes. "This is a top-level thing" — a file, a function, a loop — survives only as *depth-in-tree*, a coordinate, when it ought to be a fact with weight. SSA preserves the stratum. The tree collapses it into mere position.

That's why SSA always felt easy to me and the AST always felt lossy. It wasn't difficulty. It was that one model keeps the breadth I think in, and the other trades it for nesting I didn't ask for.

## The flat tape

This is where a side project of mine — a token *tape*, a flat structure where each token is an entry with its source offset, instead of a tree built on a stack — stopped looking like a performance hack and started looking like a conviction.

A stack-based tokenizer *is* depth-first incarnate: descend, complete, return. And it has a vice I'd always disliked without articulating: it collapses three genuinely different failures into one. A punctuation slip (a stray byte, a missing semicolon), a nesting slip (one close too few), and a guess about whether the *next* token is wrong or the *previous* one was — the stack throws the same error for all three, at the first one it trips over, at the position *it* noticed rather than the position you actually erred. It was too busy *being* the stack to *observe* the structure.

A flat tape never climbs. It records opens and closes as positioned facts and lets balance be a *question you ask afterward* — one that can report every imbalance with its offset, a repair map instead of a first-error abort. It doesn't drown when the nesting goes deep, because it never held its breath under a stack to begin with. It's a breadth-first data structure: wide, flat, positions across, depth deferred. Git does the same thing one level up — a shallow directory of text files, versioning the bytes and leaving meaning to a later pass, refusing to presume the structure. HTML5 made closing tags optional and recovers instead of faulting. Three different layers, one stance: **observe and report, don't enforce and throw.**

## Two models of a book

All of this is really an argument about a book.

The textbook model: a book is a collection of chapters, which is a collection of paragraphs, which is a collection of sentences. Pure nesting. It's the AST of a book — and it loses exactly what the AST loses. It makes "chapter" and "sentence" differ only by depth. A chapter is not merely a bigger paragraph.

The model I'll defend instead: a book is **all commodity pages, except the content and the reader**. The pages are interchangeable substrate — flat, positional, cheap, the tape. The only two things that carry irreducible meaning are the two ends of one seam: what was written (the **Def**) and who reads it (the **Use**). Everything between is queryable commodity. This is the anti-nesting model. Structure is flat and cheap; meaning lives only at the seams.

And then the move that breaks the symmetry. The reader is not the writer's mirror twin. Reading outweighs writing by an order of magnitude — my own sharper version of the old rule, **90 / &lt;10 / &lt;1**: most who arrive only read, very few write, almost none create the structure. So the design weight belongs on the Use side, not the Def side. Optimise the commodity pages for *traversal and re-reading*, not for authoring elegance.

That single asymmetry justifies the whole cluster. The brace matcher in the editor matters more than terse syntax — it serves the reader. Graceful degradation on a broken parse matters more than strict rejection — it serves the reader. The tape's "never drown, always queryable" matters more than the AST's authoring-time tidiness — it serves the reader. And it explains the line I draw on assistance: I'll accept a *typo* being flagged, because that's a byte-level fact that doesn't presume how I'm working. I reject semicolon insertion and IntelliSense, because both presume I'm depth-first — they resolve a level I deliberately left open and demand my prefix be complete enough to predict, when I'm laying down the breadth on purpose.

## Where intuition belongs

So the five dislikes were one dislike. TurboC over assembly, SSA over AST, tape over stack, commodity-pages over nested-collections, breadth-first over depth-first — not five preferences. One stance, held consistently for thirty years, finally with a vocabulary that can say it out loud: *keep structure flat and cheap so it never drowns, mark only the two seams that carry meaning, and weight everything toward the reader, because that's where the order of magnitude lives.*

Here is the thing I most want an outsider to take from this. We tell ourselves that programming advances by proof and measurement — the math and the science — and it does. But the flinch is not noise to be apologised for. It is *compression*: thirty years of pattern, felt before it can be derived. Intuition is a way of knowing that runs ahead of the proof, the same way a mathematician sees the theorem is true before the page of justification exists, the same way an experimentalist trusts an anomaly before the model explains it. The discipline isn't to suppress the flinch. It's to keep it honestly, and then do the work of finding its shape.

Mine had a shape. It was breadth-first all the way down. If you've been carrying a dislike you couldn't defend, it might be worth assuming it has a shape too — and going looking for it.

## The XML detour

This post was triggered by a conversation with Claude about its cousin — Claude Code — and its use of XML as a communication format. XML is depth-first incarnate: every element opens a scope, nesting is the structure, and a single missing close tag invalidates the whole document. The same failure mode as the stack-based parser. The same first-error abort. The same drowning under structure.

The challenge was simple: why use XML when the information is flat? A list of tool calls, a set of parameters, a sequence of results — none of these are inherently nested. XML nests them because XML nests everything. The AST of the data format. The breadth-first stance says: use a flat format, position the entries, query the relationships afterward. The tape, not the tree.

The XML defender says: nesting makes structure explicit. The breadth-first response: structure that must be enforced by a validator is structure that drowned in its own nesting. The reader pays the parsing cost on every access. The flat tape pays it once, at query time, for exactly the relationship being asked about.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). The compiler post that established the SNR framing: [The Compiler That Knew Where It Was](https://rinie.github.io/2026/05/31/compiler-snr-evolution/). The O(1) boundary principle applied to syntax: [The Brick That Sticks Out: Why {} Beats Indentation](https://rinie.github.io/2026/06/09/brick-that-sticks-out/). The Def-Use foundation: [Def-Use, Lean Pull, and Why the User Does Not Sux](https://rinie.github.io/2026/05/16/def-use-lean-pull/). The tape-tokenizer project referenced here lives at [github.com/rinie/tape-tokenizer](https://github.com/rinie/tape-tokenizer).*
