---
layout: post
title: "The Brick That Sticks Out: Why {} Beats Indentation"
date: 2026-06-09
tags: [gutenberg-semantic, boundary, programming-languages, tools, ux]
level: general
description: "Scanning for landmarks is O(1). Taking measurements is O(n). The curly brace is the brick that sticks out of the wall. Significant whitespace makes every brick flush — and hides the loose one until brick 98."
---

There is a reason experienced developers can skim a thousand lines of code in seconds and land exactly on the function they were looking for. They are not reading — they are scanning for landmarks. The `{` that opens a block. The `}` that closes it. The visual protrusion from the wall of text that says: *structure changes here*.

Scanning for landmarks is O(1). You see the landmark or you don't. Taking measurements is O(n). You count the spaces, compare against the current indent level, track the stack. The difference is not a matter of taste. It is a difference in the cost of finding where you are.

---

## The Brick That Sticks Out

Imagine inspecting a brick wall for a loose brick. You have two options.

The first: hover along the wall, eyes slightly unfocused, looking for the brick that protrudes slightly — the one that catches the light differently, that breaks the flat plane of the surface. When you find it, zoom in. Confirm it is loose. Fix it.

The second: start at brick 1, press each brick individually, measure the depth, compare against the expected depth, move to brick 2. Repeat until you find brick 98, which is the loose one. By the time you reach it you have forgotten what brick 1 felt like.

The first approach is how `{}` works. The closing brace protrudes. It is a visual landmark at a fixed position relative to the block it closes. A scope that is unexpectedly deep or shallow *looks wrong* immediately — the brace is in the wrong column, the block is the wrong shape. The anomaly is visible before you read a single character inside the block.

The second approach is how significant whitespace works. Every line is flush with the wall. The indentation that is wrong by one space is indistinguishable from the indentation that is correct — until you measure it precisely, compare it against the surrounding context, and discover that brick 98 is the problem. By then you have lost track of bricks 1 through 97.

---

## Why This Is Not Just Personal Preference

A preference for `{}` over indentation feels like aesthetics — some people like punctuation, some find it noisy. But the O(1) property is not aesthetic. It is measurable, and it applies to every layer of the toolchain that touches code.

**Parsers.** A parser that finds block boundaries by scanning for `{` and `}` can be written in a handful of state machine transitions. A parser that finds block boundaries by measuring indentation must track current indent level, maintain a stack, handle tab-versus-space ambiguity, and decide what to do when an indented line is followed by a blank line followed by a less-indented line. The significant whitespace parser is not just more complex — it has more failure modes, more edge cases, and more ways to produce a confusing error message when something is wrong.

**Diff tools.** A git diff of a `{}`-delimited file shows the changed lines and the braces that bound them. The scope of the change is visible from the diff alone — you can see which block opened, which block closed, what was added inside. A diff of a Python file shows indented lines with no structural landmarks. Whether a change is *inside* a function or *after* it requires counting spaces in the diff output.

**AI tools.** Every code assistant, including the one helping write this blog, handles `{}` delimited code more reliably than significant whitespace. The reason is the same as for parsers: the structural landmarks are explicit tokens, not measurements. Finding the end of a function in JavaScript means scanning for the matching `}`. Finding the end of a function in Python means tracking indent levels from the `def` line — which requires the surrounding context to be correct and complete.

**The loose brick test.** When something is wrong in a `{}`-delimited codebase, the wrongness usually protrudes visually before you read the logic. A missing `}` somewhere produces a cascade of alignment errors that are visible as shape anomalies. In a significant whitespace codebase, a misindented line looks exactly like a correctly indented line — until you run the code and discover that brick 98 was in the wrong scope all along.

---

## The Noise Argument Inverted

The case for significant whitespace is usually framed as a noise reduction argument: remove the `{`, `;`, and `)` clutter, and the semantic content of the code is easier to read. Less punctuation, cleaner surface.

This is the hiding-the-waterline problem from the [previous post](https://rinie.github.io/2026/06/08/hiding-the-waterline/). The punctuation is not noise. It is the structural signal — the brick that sticks out, the landmark you scan for, the boundary marker that tells every reader (human, parser, diff tool, AI) where structure changes. Removing it makes the surface cleaner and the structure invisible.

YAML's significant whitespace is the canonical example of this going wrong in practice. The indentation-as-structure model makes YAML files look cleaner than JSON. It also makes them treacherous to edit — a single misplaced space silently changes the meaning of an entire subtree, with no visual protrusion to alert you. The [Norway problem](https://rinie.github.io/2026/05/21/yaml-json-config-formats/) is not a parser quirk. It is what happens when the structural layer is invisible.

Bash's quoting rules and here-documents are another version: significant whitespace in heredocs, ambiguous word splitting, rules that depend on whether a space appears before or after a specific character in a specific context. The rules are consistent once you know them. They are not locally visible. You cannot hover and zoom — you have to know the measurement rules from memory.

---

## Where the Personal Part Lives

The O(1) landmark argument applies to tooling universally. Where personal preference enters is in the *tolerance for the trade-off*.

Some people genuinely experience `{}` as visual noise that competes with the semantic content they are trying to read. For them, Python's cleaner surface is a genuine readability improvement, and the O(1) property of braces is a cost they are willing to pay in exchange. That preference is real and not wrong.

What is worth being honest about is what is being traded. Significant whitespace trades a Gutenberg property — local, O(1) boundary detection that every tool in the chain can use — for a Semantic preference — a cleaner visual surface that one human finds easier to read. The Gutenberg property serves everyone who touches the code: the human reader, the parser, the diff tool, the AI assistant, the future maintainer who has never seen the codebase before. The Semantic preference serves the person who wrote it.

The Use signal from the broader ecosystem is consistent: formatters (Black, Prettier), linters, language servers, and AI coding tools all handle `{}`-delimited code with less complexity and fewer edge cases than significant whitespace. The brick that sticks out is the feature that makes the code inspectable at scale — by tools, by teammates, and by your future self at 11pm looking for brick 98.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Hiding the Waterline Makes You Drown Without Knowing Why](https://rinie.github.io/2026/06/08/hiding-the-waterline/) on the cost of removing structural signals, and [YAML, JSON, and the Config File That Fights Back](https://rinie.github.io/2026/05/21/yaml-json-config-formats/) on the O(1) boundary principle applied to configuration formats.*
