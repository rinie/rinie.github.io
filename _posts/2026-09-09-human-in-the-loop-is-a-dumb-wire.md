---
layout: post
title: "Human-in-the-Loop Is Not a Safety Filter. It's a Dumb Wire."
date: 2026-09-09
tags: [gutenberg-semantic, ai, human-in-the-loop, dumb-wires, resolver, architecture-freedom, grafschaft-bentheim]
level: general
description: "Grafschaft Bentheim's AI phone bot handled 380 calls in a month and never once made a decision on its own. Restricted to research, search, and summarization. Restricted to internally verified files. Restricted to whatever permissions the human it's helping already has. Restricted to a swappable model, open-source or sovereign cloud, either way. That's not caution bolted onto AI as an afterthought. It's the same four virtues already argued for wires, applied to a model instead."
---

[Grafschaft Bentheim's digitisation work](https://rinie.github.io/2026/08/24/porsche-no-gas-money/) already showed up once in this series for building the boring back-end first. The same reporting had a second thread worth its own post — how the district actually constrains its AI, and it turns out to be [the exact four-virtue shape already argued for dumb wires](https://rinie.github.io/2026/09/05/in-praise-of-dumb-wires/), applied to a model instead of a protocol.

---

## Restricted to Research, Search, and Summarisation

The district's own stated boundary: AI supports *exclusively* with research, search, and summarising information. Not decisions. The phone bot, Hörbert, handled roughly 380 calls at the IT service hotline in a single month and 245 conversations at the health department in thirty days — real volume, real usage — and its own design brief was never to answer independently past a certain point. If it can't resolve something itself, it opens a ticket for a human rather than guessing.

That's **adds nothing** from the four virtues, restated for a model instead of a wire. The AI doesn't get to have an opinion that counts as an answer. It surfaces what it found; a human decides what it means.

---

## Restricted to What's Already Verified

Hallucination risk gets contained by scope, not by hoping the model behaves: the system queries only internally verified case files, never the open web, never anything ungrounded. That's the same move [Ambiguity Is Not a Bug argued for resolvers generally](https://rinie.github.io/2026/06/22/ambiguity-is-not-a-bug/) — don't let the system answer beyond what it can actually check, and if it can't check something, say so rather than filling the gap with confidence. **Wastes nothing beyond a known, bounded cost** — the cost here isn't wasted bytes, it's wasted trust, and the district bounded it the same way a page-size choice bounds waste: by scoping the input, not by trusting the output.

---

## Restricted to Permissions Already Granted

The AI agent only ever gets access to files the specific employee it's helping already has read permission for. It doesn't get its own elevated authority. It inherits exactly the scope the human already had, narrowed to that human's session, every time. That's **reliable** in the specific sense this series has used it — fewer decisions made on the model's own initiative means fewer ways for those decisions to be wrong, because the ones that matter most (who gets to see what) were never handed to it in the first place.

---

## Restricted to a Model You Can Swap

Perplies called it *"architecture freedom"* — a municipality can run open-source models like Mistral or Llama 3 in its own data centre, or use a sovereign cloud provider, and switch between them without the surrounding workflow caring which. Confidential data never trains a public model either way. That's **swappable**, stated almost word for word the way [S3 and its competitors already proved it](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/) — and it's also [cheap Exit](https://rinie.github.io/2026/09/06/swappable-needs-a-cheap-exit/), preserved on purpose rather than lost to whichever vendor got there first.

---

None of this reads as an AI safety framework, stated this way. It reads as the same four-virtue argument this series has made about wires, storage, and allocators, aimed at a model instead. Human-in-the-loop isn't a filter bolted onto something powerful to slow it down. It's what happens when the AI itself gets held to the same standard already argued for everything else that touches a boundary — bounded, scoped, swappable, and never quietly promising more certainty than it actually has.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [In Praise of Dumb Wires](https://rinie.github.io/2026/09/05/in-praise-of-dumb-wires/) on the four virtues this post applies to a model, and [They Bought a Porsche and Had No Money Left for Gas](https://rinie.github.io/2026/08/24/porsche-no-gas-money/) on the rest of Grafschaft Bentheim's digitisation work.*

Source: [Missing Link: Digitalisierungswüste? Was die Grafschaft Bentheim besser macht](https://www.heise.de/hintergrund/Missing-Link-Digitalisierungswueste-Was-die-Grafschaft-Bentheim-besser-macht-11403797.html), heise online, August 2026.
