---
layout: post
title: "Perl 6 Promised Better. Python 3 Just Shipped."
date: 2026-08-16
tags: [gutenberg-semantic, paul-ford, perl, python, deprecation, walled-garden, tribal-def]
level: technical
description: "Paul Ford's 2015 essay for Bloomberg noted, in passing, that Perl 6 had been fifteen years in the making and still wasn't done. Python 3 shipped on schedule and spent a decade being painfully, survivably adopted anyway. Same kind of promise — a strictly better language — two completely different outcomes, and the difference is exactly the resolver this series keeps looking for."
---

Paul Ford's 2015 essay for Bloomberg, [What Is Code](https://www.bloomberg.com/graphics/2015-paul-ford-what-is-code/), ran at roughly 38,000 words — reportedly the longest single piece the magazine ever published — and inside it sits a two-sentence case study this series hasn't used yet, worth pulling out on its own. Ford, describing what happens when a programming language tries to become a better version of itself: *"changing a language is like fighting that war all over again."* His example was Perl.

---

## The Rewrite That Never Landed

Perl 5, Ford notes, was released in the mid-1990s and grew alongside the web it was built for. Perl 6 was announced in 2000 as a ground-up redesign — better in every way, a clean break from the accumulated compromises of Perl 5. By the time Ford was writing in 2015, fifteen years in, there was still no official Perl 6.

That's [the walled garden from a different angle](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/) — not Ada's committee trying to be comprehensive for every domain, but a language community trying to be comprehensive for every idea anyone had ever wanted Perl to be capable of. The rewrite kept growing in scope precisely because nothing forced it to ship. There was no existing Perl 6 install base creating pressure to converge on something shippable — only the promise of a strictly better language, indefinitely deferred, while Perl 5 kept being the thing anyone actually running production code had to use. (Perl 6 eventually did ship, years later, under the name Raku — by which point the ecosystem had moved on enough that the rename was as much an acknowledgment of a fresh start as a rebrand.)

---

## The Rewrite That Shipped Anyway

Python's own major-version transition, running roughly the same years, is the deliberate contrast — not because it was painless, but because it was survivable. Python 3 broke real things: string handling changed, `print` became a function, plenty of libraries needed real porting work. The community, as Ford notes, was watching Perl's version fracture in real time and built the transition differently on purpose.

What Python kept that Perl 6 didn't: a resolver. Python 2 and Python 3 ran side by side, supported in parallel, for over a decade. Tools like `2to3` existed specifically to bridge old code to the new syntax. Libraries could declare compatibility with both versions during the transition rather than being forced to pick a side. It was slow, and by most accounts it was more painful than anyone wanted — [the deprecation clock ran a full decade before Python 2 was finally retired](https://rinie.github.io/2026/06/07/deprecation-considered-harmful/) — but there was always a path from where you were to where you were going, walked at whatever pace your own codebase could tolerate.

Perl 6 offered no equivalent path, because there was nothing stable enough yet to resolve against. Python 3 shipped imperfect and improved in public, with a bridge back to Python 2 the whole time. Perl 6 tried to be finished before it shipped at all, and finished never quite arrived.

---

## Tribes Get a Neighborhood, Not Just a Metaphor

Ford's essay is also worth crediting for the tribal framing [this series has used since its earliest posts](https://rinie.github.io/2026/05/18/carplay-nokia-certification-tribe/) — he makes it concrete rather than abstract. Imagining the eleven million professional programmers on Earth as the population of greater Los Angeles, he sorts them by neighborhood: Mac programmers in one district, mobile in another, finance programmers in Beverly Hills, an entire county for Windows developers. *"They have different cultures, different tribal folklores"* — his own phrase — that shape how each group organises its working life, defends its tools, and treats outsiders who show up speaking a different dialect of the same underlying craft.

That's the tribal-Def pattern this series keeps finding at every scale, given an actual shape instead of just a name: not one programming culture with regional accents, but genuinely separate tribes, each with its own scriptorium, each convinced its own copying tradition is the correct one — the same pattern that showed up in [the GTK/Qt theming wars](https://rinie.github.io/2026/07/05/billy-with-opinions/) and the init-system arguments nobody outside Linux ever asked to have settled on their behalf.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Deprecation Considered Harmful](https://rinie.github.io/2026/06/07/deprecation-considered-harmful/) on why old stars fade instead of getting outlawed, [Wirth's Law Only Holds If You Recompile the Whole Iceberg](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/) on the walled garden that never resolves against anything outside itself, and [CarPlay, Nokia, and the Certification Tribe](https://rinie.github.io/2026/05/18/carplay-nokia-certification-tribe/) on tribal Def elsewhere in this series.*

Source: [What Is Code?](https://www.bloomberg.com/graphics/2015-paul-ford-what-is-code/), Paul Ford, Bloomberg Businessweek, June 11, 2015.
