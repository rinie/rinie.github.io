---
layout: post
title: "Seven Free Lunches, and Who Got the Bill"
date: 2026-11-03
tags: [gutenberg-semantic, web-development, web-sql, angular, amp, spa, free-lunch, exit, dumb-wires]
level: technical
description: "InfoWorld's seven failures of modern web tech come with seven separate lessons: consensus, migration paths, open ecosystems, native performance, Conway's Law. Read against this series they ask one question. Every pitch was a free lunch, and in every case the bill went to someone who hadn't agreed to pay it: other implementers, existing customers, publishers, end users."
---

Matthew Tyson's [seven egregious failures of modern web tech](https://www.infoworld.com/article/4237317/seven-egregious-failures-of-modern-web-tech.html) each come with their own lesson: standards need consensus, never break the migration path, stay tethered to broad use, open standards win, don't trade user performance for developer experience, don't let Conway's Law write your bundle, the server exists for a reason. Seven failures, seven lessons. Read against [the case this series has made for dumb wires](https://rinie.github.io/2026/09/05/in-praise-of-dumb-wires/), they ask one question seven times: who got the bill?

Every one of them was a free lunch. The proposer took the benefit up front, a standard, a cleaner framework, a type checker, a faster mobile page, tidier styles, autonomous teams, a native feel, and the cost landed on someone who hadn't agreed to carry it.

---

## The Bill Goes to Everyone Outside the Building

Web SQL stalled on a sentence in its own specification. The W3C's note, published in November 2010 when the work stopped, says the effort hit an impasse: every interested implementer used the same SQL backend, SQLite, and "we need multiple independent implementations to proceed along a standardisation path." The spec shows how it got there. Its section on the SQL dialect points at SQLite, pinned to version 3.6.19. The interface was defined as whatever one C library did that year, so interface and implementation were the same object, and a second vendor had nothing to implement except a copy of the first.

[S3 is the opposite design](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/): a handful of verbs with nothing in them that only Amazon can do, which is why R2 and StackIT could build against it. Per the article, Mozilla and Microsoft refused to tie the web to one library, and the developers who had built on Web SQL were left with IndexedDB.

Flow ran the same play inside one company. According to the article it was built for Facebook's own environment; outside Meta it needed a heavy background server, crashed on Windows, and ignored the wider ecosystem. [That is the Borg pattern](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/): an internal iceberg that never had to resolve against a stranger, because nothing inside the building was one. TypeScript, which [compiles into the substrate instead of building beside it](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/), took the ground.

---

## The Bill Goes to the People Already Inside

Angular 2 billed the customers it already had. Per the article, Google announced a complete, non-backward-compatible rewrite with no clear migration path for large enterprise codebases, and React walked through the opening and stayed. It is [the Perl 6 shape](https://rinie.github.io/2026/08/16/perl6-python3-walled-garden/): a better future announced, no bridge from the present. Python 3 shipped a bridge, and [512e carried old disk assumptions across a sector-size change](https://rinie.github.io/2026/08/28/the-page-size-can-evolve-too/). Angular's users got exactly one cheap way out, and it led to a different framework.

AMP billed publishers, and went after the one thing that could have refused payment. In the article's account, Google pushed publishers onto a stripped-down subset of HTML under threat to their search position. That is pricing Exit up: a toll booth on the only road, where leaving costs more than paying. [Swappable Needs a Cheap Exit](https://rinie.github.io/2026/09/06/swappable-needs-a-cheap-exit/) described the mechanism from the other side. The article's verdict is that AMP was routed around anyway.

---

## The Bill Goes to the Person Holding the Phone

The last three never reached a standards table or a customer contract. They billed the one party with no say at all, and the article says so outright about the first: "the end-user always pays the tax."

Runtime CSS-in-JS handed CSS parsing, which the browser does natively and fast, to the single-threaded JavaScript engine. The developer got colocated styles. The user got slower loads and a hotter phone. That is [the log cabin](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/): rebuilding at runtime what the substrate already provides. The way back, per the article, runs through native CSS and zero-runtime compilers, which is resolving against the platform again.

Micro front ends solved an organisational problem, independent teams, by shipping the org chart to the browser: several bundles, duplicate dependencies, sometimes competing framework versions on one page. That is the shipping-container failure of bundled runtimes inside a single page, with [Conway's Law](https://rinie.github.io/2026/05/28/why-ipv6-def-push-failed/) written into the artifact. The article's remedy, ES modules and buildless infrastructure, is [the import-map argument](https://rinie.github.io/2026/07/29/java-reversed-hierarchy-forgot-resolver/): name the thing, resolve it once, against a shared substrate.

Client-side-only SPAs put state on both sides, and the article's diagnosis is that replicated state brings distributed computing with it. A second copy of state is a second source of truth, and [several sources of truth can disagree](https://rinie.github.io/2026/09/10/four-shapes-a-boundary-can-take/); that is what the freedom costs. The article's shorthand, a smart client and a dumb server, needs separating from this series' dumb wires, because it sounds like the opposite claim. A dumb wire is an interface that doesn't interpret what crosses it, and the wire in an SPA, HTTP carrying JSON, stayed exactly that. What failed was what got placed at either end: business logic and a replicated copy of state in the client, plus a layer translating between the two copies. The wire was fine. The free lunch was the second source of truth.

---

The article ends by telling developers to check how implementations are actually going rather than how the plan says they should. That is [going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) without the name, and for these seven it reduces to one question that was answerable before any of them failed: who is carrying the cost? The proposer's slides won't show it. The person carrying it will.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [In Praise of Dumb Wires](https://rinie.github.io/2026/09/05/in-praise-of-dumb-wires/) on the free lunch a smart wire pretends to offer, [Swappable Needs a Cheap Exit](https://rinie.github.io/2026/09/06/swappable-needs-a-cheap-exit/) on what coercion like AMP removes, and [Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on checking who is actually paying.*

Sources: ["Seven egregious failures of modern web tech"](https://www.infoworld.com/article/4237317/seven-egregious-failures-of-modern-web-tech.html), Matthew Tyson, InfoWorld, November 2, 2026; [Web SQL Database](https://www.w3.org/TR/webdatabase/), W3C Working Group Note, 18 November 2010.
