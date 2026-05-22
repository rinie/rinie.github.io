# rinie.github.io

A blog about software architecture, systems thinking, and the boundary between physical and logical layers.
Exploring being a Weak link willing to **learn** that is Listen to actual Use **feedback**.

Published at [rinie.github.io](https://rinie.github.io).

---

## The Series

Eight posts, one coherent framework applied at every layer from printing presses to semicolons.

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

Posts are drip-fed one per day via Jekyll's scheduled publishing. No configuration needed — GitHub Pages publishes each post automatically when its date arrives at midnight UTC (02:00 Netherlands time).

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
- No build step required locally — GitHub builds on push

---

## Adding a Post

Create a file in `_posts/` named `YYYY-MM-DD-title.md` with front matter:

```markdown
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
---

Post content here.
```

Push to `main`. GitHub Pages builds and publishes automatically. Future-dated posts publish at midnight UTC on their date without any further action.

---

## Assets

The iceberg diagram (`assets/img/gutenberg_semantic_iceberg.svg`) illustrates the core framework — a Unix/Node.js stack with a clean Gutenberg/Semantic boundary at `fread/libc` versus a Java/.NET stack where semantic noise permeates every layer. Open in any browser or vector editor.
