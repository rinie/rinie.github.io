---
layout: post
title: "Deprecation Considered Harmful"
date: 2026-06-07
tags: [deprecation, python, javascript, utf-8, gutenberg-semantic, dual-track]
level: technical
description: "Deprecation is only warranted for security reasons. Old stars fade — you do not outlaw them. Python 3, .mjs, and UTF-16 are what happens when the future expert is arrogant about the past. Dijkstra deprecated goto correctly. The dual-track strategy is how you move fast without breaking things."
---

*The title echoes Dijkstra's 1968 letter "Go To Statement Considered Harmful" — deliberately. Dijkstra deprecated goto correctly. The programming world migrated voluntarily over decades. Goto still compiles in C today. Nobody uses it for new code. The old star faded. Nobody outlawed it. That is the model.*

---

## Two Kinds of Expert, Two Kinds of Wrong

There are two failure modes for technical experts, and they are mirror images of each other.

**The static expert** is blind to the future. They optimise for today's stack, collapse the seams between layers, and cannot absorb evolution. When the platform improves, their tightly coupled code fights the improvement instead of harvesting it. The 1988 assembly programmer who hand-tuned every instruction for the 68000 and could not port to the 68040. The 2005 enterprise architect who hardcoded SOAP into every integration and spent a decade paying the REST migration tax.

**The future expert** is arrogant about the past. They can see the better path clearly, and they assume that everyone who has not taken it yet is simply wrong. They deprecate working things not because they are broken but because the new model is cleaner. They impose migration cost on users who have stable, working code — users who are paying a tax the expert designed but is not paying themselves.

Both experts are wrong. The static expert loses to evolution. The future expert mistakes their own aesthetic preferences for security vulnerabilities.

**Deprecation is only warranted for security reasons.** An API with a known vulnerability must go — leaving it open causes active harm. Everything else should follow the dual-track strategy: ship the new thing, make it clearly better, let the old thing fade. Old stars do not need to be outlawed. They fade when the new star shines brighter.

---

## Dijkstra Got It Right

In 1968, Edsger Dijkstra wrote a short letter to the Communications of the ACM arguing that the `goto` statement was harmful to program clarity and should be avoided. He did not file a compiler bug. He did not propose removing `goto` from C. He made the argument, showed the better alternative (structured control flow with `if`, `while`, `for`), and let the ecosystem decide.

The ecosystem decided over the next twenty years. Structured programming won. `goto` usage declined to near zero in new code. It still compiles in C, C++, and most languages that had it. Nobody campaigns to remove it. The old star faded on its own.

This is the correct deprecation model. The new approach was better. The evidence was presented. The migration happened voluntarily, pulled by quality rather than pushed by breaking changes. The transition took decades and cost almost nothing — because nobody forced it.

---

## The 16-Bit Bet: The Root of Two Migrations

In the early 1990s, Python 2 and JavaScript made the same bet independently: Unicode was coming, Unicode meant 16-bit characters, therefore the internal string representation should be 16-bit (UCS-2, then UTF-16). One character, two bytes, done. Clean. Semantic.

The bet was wrong in three compounding ways.

**16-bit was not enough.** UCS-2 covered the Basic Multilingual Plane but not the full Unicode range. When Unicode expanded beyond U+FFFF — emoji, historical scripts, mathematical symbols — UCS-2 had to become UTF-16 with surrogate pairs: two 16-bit units for characters outside the BMP. The "clean" fixed-width encoding became variable-width anyway, with the worst properties of both: you cannot index by character position in O(1), you cannot find boundaries without scanning, and surrogate pairs can be split at every API boundary.

**ASCII compatibility was destroyed.** A Python 2 ASCII string and a Python 2 Unicode string were different types. Every function had to decide which type it accepted. Every boundary between ASCII-world code and Unicode-world code was an explicit conversion point. The `UnicodeDecodeError` was the Gutenberg/Semantic boundary failing loudly — the physical byte representation was incompatible with the assumed encoding.

**The live wound in JavaScript.** JavaScript strings are UTF-16 internally. `'😀'.length` is 2, not 1 — the emoji is a surrogate pair. `charCodeAt(0)` gives you the high surrogate, not the code point. Every string operation written before ES6 assumes UCS-2 and silently breaks on emoji. ES6 added `codePointAt()`, `String.fromCodePoint()`, and the `/u` regex flag to paper over the problem, but the underlying UTF-16 representation is forever. You cannot fix it without breaking every JavaScript string ever written.

**UTF-8 avoided all of this** because Pike and Thompson, in a New Jersey diner in 1992, made three correct choices:

- ASCII bytes stay ASCII bytes — O(1) backward compatibility with every existing Unix tool
- Self-synchronizing — find any character boundary by scanning at most 3 bytes forward, no global state required
- Byte-order independent — no BOM, no endianness problem, no big-endian/little-endian variants

The O(1) boundary property is the key Gutenberg insight. UTF-16 requires you to scan from the start of a string to know whether you are at a character boundary — a `0xDC00` byte might be the start of a character or the second half of a surrogate pair, and you cannot tell without context. UTF-8 continuation bytes always start with `10xxxxxx`. You can find your position locally, without context, in constant time.

The "clean" 16-bit solution tried to make the Gutenberg layer (the byte representation) match the Semantic layer (the character model) directly — one character, one fixed-size unit. UTF-8 accepted the Gutenberg reality (bytes are bytes, characters are variable width) and put the complexity in the right place: a local, O(1) encoding that any Gutenberg layer can handle without semantic knowledge.

Gutenberg 2.0 — the Unix bytestream — was already built on ASCII bytes. UTF-8 extended that stream to carry the full Unicode semantic layer without breaking the Gutenberg foundation. UTF-16 tried to replace the foundation and paid the price every time it met the existing bytestream world — which is everywhere, always.

Python 3 fixed the 16-bit bet but paid the migration tax in ecosystem breakage. JavaScript has not fixed it and still pays daily in every codebase that handles emoji, Arabic, or any character outside the BMP. The deprecation was the symptom. The wrong Gutenberg bet was the disease.

---

## .mjs: The Filename That Should Have Been package.json

Node.js faced the same problem when ES modules arrived alongside CommonJS `require()`. Two module systems, one runtime. Something had to signal which system a file used.

The solution chosen: `.mjs` for ES modules, `.cjs` for CommonJS, `.js` for whichever the `package.json` `"type"` field declared. Years of confusion, dual-format packages, tooling that did not understand the new extensions, and a migration that is still incomplete half a decade later.

The mistake was putting semantic meaning (module format) into the Gutenberg address (the filename extension). The filename is a physical artifact — a Gutenberg address. It identifies the file. It does not describe the file's semantic type. That declaration belongs in the resolver — `package.json` — which is the external mapping from Gutenberg address to semantic meaning. This is exactly the DNS model: the hostname (semantic) maps to the IP address (Gutenberg) via an external resolver, not by encoding the IP address into the hostname.

`.mjs` is the equivalent of encoding the server's IP address into the domain name. It works. It is the wrong layer. The correct solution — `"type": "module"` in `package.json` — was available from the start. The ecosystem would have been cleaner if `.mjs` had never existed and the resolver had been trusted to do its job from day one.

---

## Retina Displays: Four Times the Pixels, One Page

The Retina display is a clean example of the dual-track strategy applied to hardware.

A Retina display has four times the physical pixels of a standard display — a Gutenberg improvement. The same number of logical points (the Semantic layer), four times the physical dots rendering each one. Your webpage from 2019 displays on a Retina screen without any changes — the Gutenberg layer improved, the semantic layer harvested the improvement for free.

For images, the web provides `srcset` and `device-pixel-ratio` — the semantic layer declares "I want this image at 2x if available", the Gutenberg layer (the browser, the screen) decides how many physical pixels that is. The high-resolution asset is the new star. The standard-resolution asset is the old star. Both work. The user on a Retina display gets the better experience. The user on a standard display gets the same experience they always had. Nobody's page broke.

This is what "move fast without breaking things" actually means. The Gutenberg layer improved. The semantic layer did not change. The old path stayed open. The new path was made available. Users on better hardware collected the improvement for free.

---

## Your 2019 Webpage Should Still Display

The web's backward compatibility guarantee is one of the most important engineering commitments in computing history. HTML from 1996 still renders. `document.write()` still works. `<table>` layout still displays, even though CSS Grid is the correct answer today. The old star fades in usage; it does not get outlawed.

Stale links are the allowed failure mode. Your 2019 page may have links that return 404 — URLs conflate identity with location, and resources move. The link is stale. The page is not broken. The browser renders what it can. The missing resource is a gap, not a parse error. This is the correct failure mode: local, bounded, recoverable.

**XML fails this test completely.** A well-formed XML document where one page is missing — even a page that says "this page intentionally left blank" — can invalidate the entire document. If that missing page contains a closing tag, a namespace declaration, or a DTD reference that the rest of the document depends on, the XML parser refuses to process any of it. The document is either fully valid or entirely broken. There is no partial delivery. There is no graceful degradation.

This is the Gutenberg page model failing. If you request pages 1–14 and 16–300 of a book and page 15 is missing, you still have a readable book with one gap. If the book is XML and page 15 contains a closing tag that page 200 depends on, you have nothing. The format that couples every page to every other page cannot survive the physical reality that pages sometimes go missing.

HTML succeeds where XML fails because HTML has always had an error recovery model. The browser encounters a malformed tag and does its best. The page renders with a gap, not an error. The Gutenberg layer (the byte stream, the HTTP response) delivered what it could. The semantic layer (the rendered page) reflects that partial delivery faithfully. The user sees something, not nothing.

---

## The Dual-Track Principle

The correct deprecation model is not "announce a deadline and break the old thing." It is:

1. **Ship the new thing.** Make it clearly better — faster, safer, more expressive, more correct.
2. **Let the old thing keep working.** Do not break existing code. Do not fail builds. Do not make the migration mandatory.
3. **Signal the preference.** A linter warning, a documentation note, a deprecation annotation that informs without breaking.
4. **Wait for the pull.** Developers migrate when the new thing is better enough to justify the cost. That pull is stronger and faster than any push.
5. **Security exception only.** If the old path causes active harm — a known vulnerability, a cryptographic weakness, an attack vector — close it. Otherwise, let it fade.

Dijkstra deprecated goto this way. UTF-8 deprecated UTF-16 this way — it did not break UTF-16, it just made itself available and let the ecosystem decide. Retina displays deprecated standard resolution this way — the old images still render, the new ones look better.

Python 3 deprecated Python 2 the wrong way — broke the ecosystem, imposed the migration, split the community, and delayed adoption by a decade. The future expert was right that Python 3 was better. They were wrong that they had the right to make the migration mandatory.

**The static expert is blind to the future. The future expert is arrogant about the past. Both lose to the dual-track strategy — which is also the only way to actually move fast without breaking things.**

Old stars fade. New stars shine. The universe does not need to outlaw the old ones.

---

## Coda: Linux Just Kept Moving

Nobody deprecated `wchar_t`. Nobody broke Windows, Java, or JavaScript string APIs. The 16-bit world still exists, still compiles, still runs. Old star, not outlawed.

But Linux — the kernel, the shell, the entire Unix toolchain — just kept working on 8-bit bytes and never changed. When UTF-8 arrived it slotted in perfectly: every `char*`, every `read()`, every pipe, every file descriptor already handled bytes. UTF-8 is bytes. The Gutenberg layer was already correct. The semantic layer — Unicode, international text, emoji — arrived as a free upgrade. No migration. No deprecation. No breaking change. Just a new encoding that happened to be compatible with the existing bytestream infrastructure.

The result thirty years later:

- **Linux** runs UTF-8 everywhere, faster and simpler than any 16-bit system, because the Gutenberg layer never changed
- **Windows** carries the UTF-16 representation through every API, every string, every filename — `wchar_t` everywhere, surrogate pairs in Explorer, `MultiByteToWideChar` at every boundary crossing
- **Python 3** fixed its 16-bit bet but paid a decade of migration tax to get there
- **JavaScript** has not fixed it and pays daily in every emoji-handling bug, every `string.length` that counts surrogates instead of characters

Linux did not win by deprecating anything. It won by having the right Gutenberg layer from the start — the 8-bit bytestream — and letting UTF-8 evolve on top of it naturally. The new star (UTF-8, international text, the full Unicode range) shines brightly. The old infrastructure (8-bit bytes, ASCII tools, Unix pipes) never had to change. Both still work. The semantic layer improved for free.

The 16-bit bet was the wrong Gutenberg choice. Linux never made it. Linux never had to unmake it. Linux kept moving.

That is the dual-track strategy working at operating system scale, across thirty years, without a single forced migration. Not because Linux deprecated the old path. Because Linux never left the right path in the first place.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) on accepting the 10% overhead to collect future improvements, [DuckDB: The Gutenberg/Semantic Model Done Right](https://rinie.github.io/2026/05/22/duckdb-gutenberg-semantic/) on the dual-track strategy in practice, and [Working on the Same Page](https://rinie.github.io/2026/06/06/working-on-the-same-page/) on why clean seams let specialists go deep.*
