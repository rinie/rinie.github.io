---
layout: post
title: "MTOM Fixed the Bloat. It Never Fixed Page 15."
date: 2026-08-30
tags: [gutenberg-semantic, xml, docbook, ooxml, zip, mtom, xop, base64, resolver, diamond]
level: technical
description: "DocBook inlines images as base64 text inside the XML stream — a third bigger than the original bytes, and unreachable except by parsing the whole document. OOXML stores images as raw PNGs in a ZIP, addressable by name through a central directory. MTOM fixed SOAP's version of the bloat problem in 2005 and never touched the deeper one: it optimizes one message's attachments, not random access into a document's structure."
---

DocBook, and most XML formats that need to carry an image, do it the same way: encode the binary as base64 text and drop it inline as element content. It works, and it costs exactly what you'd expect. Base64 inflates binary data by roughly a third for zero semantic benefit — and the sharper cost is structural. [The image bytes are now fused into the same linear character stream as the document's markup](https://rinie.github.io/2026/08/26/the-diamond-moves-it-doesnt-disappear/), which means there's no way to reach just the picture. You parse and re-serialize the entire document to touch any part of it, because the picture was never stored at a byte offset — it's text, sequential, interleaved with everything around it.

---

## The ZIP Answer

OOXML — the format behind `.docx`, `.pptx`, `.xlsx` — solves this by refusing to store binary as text at all. The file is a ZIP archive. Structure lives in separate XML parts. Images are raw PNG or JPEG bytes, untouched, sitting as their own named entries. A ZIP archive carries a central directory at the end, giving O(1) lookup of any named part by byte offset without touching anything else in the file. You can extract `image3.png` from a hundred-megabyte presentation without decoding a single byte of `slide12.xml`.

That's [the same shape this series keeps finding at every layer](https://rinie.github.io/2026/08/29/one-lookup-per-book/) — physical binary content and semantic structure kept in separate, independently addressable containers, resolved through a lookup table instead of fused into one stream a consumer has to walk from the start. DocBook's inline base64 is the diamond. OOXML's ZIP-plus-central-directory is the hourglass, wearing a file-format costume instead of a database schema.

---

## MTOM Solved Half the Problem, in 2005

The XML web-services world hit this exact wall independently and built a real fix for it: MTOM, a W3C Recommendation finalised in 2005, paired with XOP (XML-binary Optimized Packaging). Instead of base64-encoding a binary payload inline in a SOAP message, MTOM pulls it out into a separate MIME part and leaves an `<xop:include>` reference in its place — the same move OOXML makes, arrived at by a completely different standards body solving what looked like a completely different problem.

It's worth being precise about what MTOM actually fixed, because it's narrower than it sounds. The specification describes MTOM's optimisation as *"a hop-by-hop contract between one SOAP node and the next"* — it operates at the granularity of one message and its attachments. It never touches the structure of the XML document carrying that message. There's no ZIP-style central directory anywhere in MTOM. The SOAP envelope is still one sequential XML stream, exactly as parseable-from-the-start as it always was; the only thing that changed is that the binary payload riding alongside it no longer has to be text.

That means MTOM fixes the DocBook problem's bloat half and leaves the addressability half completely untouched. If the document itself is large — a long report, a big set of structured records — and you want just page 15, MTOM gives you no help at all. The binary attachments are reachable by reference. The document's own internal sections are not. You'd still have to parse the whole XML infoset from byte zero to find where page 15 begins, because nothing in MTOM ever asked that question. It solved the problem SOAP happened to have — bloated messages — without solving the more general problem this series keeps finding underneath it: a format that fuses structure and content into one stream has no page-break table, no matter how cleanly it handles the pictures.

---

## Two Different Questions, Only One Format Answers Both

OOXML's ZIP container answers both questions at once, because the central directory doesn't care whether an entry happens to be binary or XML — `image3.png` and `slide12.xml` are both just named parts, both reachable in O(1) by the same mechanism. MTOM only ever answers the binary question. The structural one — can I reach an arbitrary section of the document itself without parsing everything before it — was never in scope, because SOAP messages are typically small enough that nobody building the spec needed to ask it.

That's the honest lesson in comparing the two: fixing the visible cost (bloat, in MTOM's case; a third larger payloads) doesn't automatically fix the deeper architectural one (addressability). A format can solve one and quietly leave the other exactly where it started, and the only way to notice is to ask a question neither spec was actually trying to answer — not "how big is this," but "how do I reach just the part I need."

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [The Diamond Moves, It Doesn't Disappear](https://rinie.github.io/2026/08/26/the-diamond-moves-it-doesnt-disappear/) on structure and content fused into one artifact, and [One DNS Lookup Per Book](https://rinie.github.io/2026/08/29/one-lookup-per-book/) on resolving to a named part instead of parsing sequentially.*

Source: [SOAP Message Transmission Optimization Mechanism](https://www.w3.org/TR/soap12-mtom/), W3C Recommendation, January 25, 2005.
