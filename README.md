# rinie.github.io

A blog about software architecture, systems thinking, and the boundary between physical and logical layers.
Exploring being a Weak link willing to **learn** that is Listen to actual Use **feedback**.

Published at [rinie.github.io](https://rinie.github.io).

---

## The Framework Visualised

![Gutenberg vs Semantic iceberg](assets/img/gutenberg_semantic_iceberg.svg)

Left iceberg: Unix/Node.js with a clean boundary at `fread/libc` — the semantic top ~20% is portable and moves freely to any cloud zone. Right iceberg: Java/.NET where semantic noise permeates every layer and the whole stack must move together.

---

## The Series

Twenty-seven posts, one coherent framework applied at every layer from printing presses to deployment practice.

| # | Post | Date | Core idea |
|---|---|---|---|
| 1 | [The Gutenberg/Semantic Model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) | May 14 | The foundational framework |
| 2 | [Def-Use, Lean Pull, and Why the User Does Not Sux](https://rinie.github.io/2026/05/16/def-use-lean-pull/) | May 16 | The human feedback dimension |
| 3 | [MDI versus Tabs: Why Microsoft Kept Getting It Wrong](https://rinie.github.io/2026/05/17/mdi-versus-tabs/) | May 17 | Window management as tribal Def |
| 4 | [CarPlay, Nokia, and the Certification Tribe](https://rinie.github.io/2026/05/18/carplay-nokia-certification-tribe/) | May 18 | Certification as tribal weapon |
| 5 | [Sonos, Intel, and Apple: When the Tribe Breaks the Product](https://rinie.github.io/2026/05/19/sonos-intel-apple-tribe/) | May 19 | Gutenberg tribe claiming semantic layer |
| 6 | [The Linux Paradox: A Gutenberg Kernel and a Semantic Desktop](https://rinie.github.io/2026/05/20/linux-kernel-desktop-paradox/) | May 20 | Use oracle versus no oracle |
| 7 | [YAML, JSON, and the Config File That Fights Back](https://rinie.github.io/2026/05/21/yaml-json-config-formats/) | May 21 | O(1) boundary principle |
| 8 | [DuckDB: The Gutenberg/Semantic Model Done Right](https://rinie.github.io/2026/05/22/duckdb-gutenberg-semantic/) | May 22 | The model done right + dual-track strategy |
| 9 | [Exceptions, Result/Option, and HTTP: Error Handling as Def-Use](https://rinie.github.io/2026/05/23/exceptions-result-option-http/) | May 23 | Errors as values versus control flow hijacking |
| 10 | [Your Music Survived Six Formats. Why Did Your Bank Account Number Stay Behind?](https://rinie.github.io/2026/05/24/music-survived-six-formats/) | May 24 | Semantic identity versus Gutenberg carrier |
| 11 | [Your Email Address Is Hostage. It Does Not Have to Be.](https://rinie.github.io/2026/05/25/email-address-hostage/) | May 25 | Own your domain, make the carrier replaceable |
| 12 | [The Boundary Has a Lifecycle](https://rinie.github.io/2026/05/26/boundary-lifecycle/) | May 26 | From Unix portability to WebAssembly |
| 13 | [UUIDs Are Not Names](https://rinie.github.io/2026/05/27/uuids-are-not-names/) | May 27 | Gutenberg identifiers leaking into the semantic layer |
| 14 | [Why IPv6's Def-Push Failed: NAT Was Not a Hack](https://rinie.github.io/2026/05/28/why-ipv6-def-push-failed/) | May 28 | Chesterton's Fence and load-bearing boundaries |
| 15 | [Competition Is Use-Pull. Monopoly Is Def-Push. Government Is Both.](https://rinie.github.io/2026/05/29/competition-use-pull-monopoly-def-push/) | May 29 | Exit as the Use signal transmission mechanism |
| 16 | [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) | May 30 | Portability as a basket option on unknown future improvements |
| 17 | [The Compiler That Knew Where It Was](https://rinie.github.io/2026/05/31/compiler-snr-evolution/) | May 31 | SNR in IRs — why keeping `{}` structure is free information the flat CFG destroys |
| 18 | [Worse Is Better Because the Gap Is Where Evolution Happens](https://rinie.github.io/2026/06/01/worse-is-better-gap-evolution/) | Jun 1 | Gabriel was right about worse beating better, wrong about why |
| 19 | [Ten Users Saying It Sux Means It Sux](https://rinie.github.io/2026/06/02/ten-users-saying-it-sux/) | Jun 2 | 90/<10/<1 population scaling and the Def-Push distortion |
| 20 | [Your Lights Don't Know Your Name](https://rinie.github.io/2026/06/03/bluetooth-matter-rdf-naming/) | Jun 3 | Bluetooth, Matter, and RDF all push Gutenberg identifiers across the boundary |
| 21 | [The Postman Reads the Envelope, Not the Letter](https://rinie.github.io/2026/06/04/postman-reads-envelope/) | Jun 4 | Books, pages, and Gutenberg 2.1 for a general audience |
| 22 | [The Postman Does Not Read the Letter: How Gutenberg 2.1 Bounds Complexity](https://rinie.github.io/2026/06/05/postman-does-not-read-letter/) | Jun 5 | DNS once, routing per page, SNI/ECH — the boundary getting cleaner |
| 23 | [Working on the Same Page](https://rinie.github.io/2026/06/06/working-on-the-same-page/) | Jun 6 | Web craft boundaries — HTML, CSS, JS, HTTP as clean Def-Use seams |
| 24 | [Deprecation Considered Harmful](https://rinie.github.io/2026/06/07/deprecation-considered-harmful/) | Jun 7 | Old stars fade. You do not outlaw them. Python 3, UTF-16, and the 16-bit bet. |
| 25 | [Hiding the Waterline Makes You Drown Without Knowing Why](https://rinie.github.io/2026/06/08/hiding-the-waterline/) | Jun 8 | Legibility at the boundary is a feature, not a leak |
| 26 | [The Brick That Sticks Out: Why {} Beats Indentation](https://rinie.github.io/2026/06/09/brick-that-sticks-out/) | Jun 9 | Scanning for landmarks is O(1). Taking measurements is O(n). |
| 27 | [Revisiting the Waterline: Small Fixes, Five Years Later](https://rinie.github.io/2026/06/10/revisiting-the-waterline/) | Jun 10 | Platform drift, LTS cadence, and why you never deploy on Fridays |

Posts publish one per day via Jekyll's scheduled publishing. GitHub Pages publishes each post automatically when its date arrives at midnight UTC (02:00 Netherlands time), triggered by a daily GitHub Actions workflow.

---

## The Core Framework

**The Gutenberg layer** is physical and positional — bytes, blocks, pages, frames, IP addresses, sector offsets. Position and size matter. The medium is part of the artifact.

**The Semantic layer** is logical and meaningful — characters, words, chapters, hostnames, messages, rows. Hierarchy and meaning matter. Content is independent of the medium.

The name comes from Gutenberg's printing press: a physical process that fixes semantic content (text) onto a physical artifact (a page) at a specific position (a folio). The page number is Gutenberg. The chapter title is Semantic.

**Gutenberg 2.0** — Unix, TCP/IP, virtual memory. The bytestream abstraction hides the physical medium. Everything is a file.

**Gutenberg 2.1** — bytestream + UTF-8 + git. Portability extended to text and to software itself. Clone a repo, run on any machine.

The best systems isolate the Gutenberg/Semantic boundary in one place — a parser, a codec, a DNS resolver — and keep it clean.

---

## Stack

- [Jekyll](https://jekyllrb.com) with the Minima theme
- Hosted on [GitHub Pages](https://pages.github.com)
- Posts written in Markdown
- Daily publish via GitHub Actions (`.github/workflows/daily-publish.yml`)
- No build step required locally — GitHub builds on push

---

## Adding a Post

Create a file in `_posts/` named `YYYY-MM-DD-title.md` with front matter:

```markdown
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
tags: [tag1, tag2]
level: general
description: "One or two sentence description for the index."
---

Post content here.
```

Push to `main`. GitHub Pages builds and publishes automatically. Future-dated posts publish at midnight UTC on their date without any further action.

---

## Assets

The iceberg diagram (`assets/img/gutenberg_semantic_iceberg.svg`) illustrates the core framework. Open in any browser or vector editor.
