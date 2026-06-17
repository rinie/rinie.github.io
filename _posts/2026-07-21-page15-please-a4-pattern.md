---
layout: post
title: "Just Page 15, Please: The A4 Pattern and Why You Need Pages"
date: 2026-07-21
tags: [gutenberg-semantic, pages, tar, zip, parquet, resolver, indexes]
level: technical
description: "tar is stuck in the tape era — read forward or read nothing. zip added a directory at the end and became the container format Office still uses today, XML and all. Parquet does the same trick over the network: ask for the footer, then ask only for the row groups you need. You wrote a chapter. I want to read page 15. That gap is why pages exist."
---

You wrote a chapter. I want to read page 15.

That sentence is the entire argument. A chapter is a semantic unit — it has a beginning, an argument, a shape, and it doesn't end until the author says it ends. A page is a Gutenberg unit — fixed size, numbered, indifferent to whether the sentence it's holding finishes or gets cut off mid-thought and continues on the next one. You can't print "two-thirds of a chapter." You can print page 15. The reason you can is that someone, long before computers, built an index — page numbers — that lets you address a position in the book without reading the whole book first.

Three file formats make this same argument with bytes instead of paper, and one of them gets it wrong on purpose, for a reason that used to be correct.

---

## tar Is Stuck in the Tape Era

`tar` stands for *tape archive*, and the format is not flawed — it is a faithful, honest description of what magnetic tape allows. A tape drive has a read head and a reel. It can move forward. Moving backward means rewinding, which is slow, and seeking to an arbitrary position means rewinding to the start and reading forward again until you get there. There is no "jump to position 4,000,000" on tape. There is only "you are here, and here is what comes next."

So tar has no index. Files are concatenated one after another, each preceded by a small header — name, size, permissions — and then the file's bytes, and then the next header, and so on, until the tape ends. To find the third file in a tar archive, you read the first header, skip its declared size, read the second header, skip its size, and arrive at the third. To find the *last* file, you do this all the way to the end. There is no faster way, because on tape there genuinely was no faster way. The format is correct for its medium. It has simply outlived the medium.

Put a tar file on S3, and this becomes a real cost. Object storage supports random access — you can ask for byte 50,000,000 through byte 50,001,000 without touching anything before or after. tar doesn't know this is possible, because tar was finished being designed before it was true. Reading file 400 out of 500 from a tar archive on S3 still means walking sequentially through headers from the start, the same as it would on a tape drive in 1979, because nobody told the format that the rules changed.

---

## zip Put the Index Where You'd Actually Look For It

`zip` solves exactly this, with one structural decision: a **central directory** at the *end* of the file. The central directory is a small table — filename, compressed size, uncompressed size, and crucially, the byte offset where that file's data begins. To read file 400 of 500 from a zip archive, you read the last few hundred bytes (the central directory, whose own location is recorded in an even smaller record at the very end), look up file 400's offset, and seek directly there. No walking through 399 other files first.

This is the page-number trick, applied to a binary container. The central directory is the table of contents. The byte offset is the page number. The reason it's at the *end* rather than the front is itself worth noticing: when you're writing a zip file, you don't know all the offsets until you've written all the files, so the index has to be built last — but readers don't mind, because reading the last few hundred bytes of a file you can seek into is cheap regardless of where those bytes happen to sit.

zip's central directory is also why it became the container format of choice for something that, on its surface, has nothing to do with archiving: Microsoft Office. A `.docx`, `.xlsx`, or `.pptx` file is a zip archive, full stop — open one with any zip tool and you'll find `word/document.xml`, `[Content_Types].xml`, `docProps/core.xml`, and a handful of other named parts, each one addressable through the same central directory mechanism any zip reader already knows how to use. Office didn't invent a new container. It borrowed the one that already solved "give me named, randomly-addressable parts inside one file," and then used XML for the parts themselves — structure and meaning, the semantic layer, written by a format suited to expressing it, sitting inside a container format suited to finding it.

This is also a small, quiet correction to "XML lost." XML didn't vanish — it retreated to a role it's actually good at: describing structured content within a part that something else (zip's directory) handles finding and addressing. XML's failure, where it failed, was being asked to *also* do zip's job — to be both the content format and the navigation index in formats like raw XML documents and early web-services payloads, where there was no separate, page-oriented container underneath it doing the addressing for free.

---

## Parquet Does the Same Trick Over the Network

Apache Parquet takes the central-directory idea and runs it one layer further: instead of a directory just listing file *names*, Parquet's footer lists statistics about the *content* — for every row group, the minimum and maximum values in every column, the byte offset where that row group starts, and its compressed length.

A query engine reading a multi-gigabyte Parquet file sitting in S3 does not download the file. It does one small HTTP range request for the last few kilobytes — the footer — and reads it. The footer tells the engine, for instance, that row group 12 has a `date` column ranging from March to April, and row group 40 has a `date` column ranging from September to October. If the query asks for July, the engine already knows, without touching a single byte of actual data, that row groups 12 and 40 are irrelevant. It then issues range requests for exactly the row groups whose statistics overlap July, and nothing else.

This is page addressing carried all the way up through the file format and out across the network protocol. The byte range *is* the page. The HTTP `Range` header is the mechanism that lets you say "give me page 15" to a server that has never heard of your chapter, your book, or your argument — only of byte offsets, which is exactly the layer it should be operating at.

---

## The A4 Pattern

A4 paper doesn't know what's printed on it. It has a fixed size, and the printer fills it, and when it's full a new page starts, mid-sentence if necessary, mid-paragraph, sometimes mid-word with a hyphen. The page is the Gutenberg unit. The chapter is the semantic unit. They don't share a boundary, and they were never supposed to — the printer's job is to keep producing same-sized pages regardless of what the author's sentences are doing, and the page numbers are the resolver that lets a reader say "page 15" and get the same fifteenth page every time, without anyone needing to agree in advance on where the chapters fall.

tar refuses this pattern — every read is a chapter-by-chapter walk from the beginning, because tape never offered anything better. zip adopts it for files, with a directory that turns "find file 400" into "look up an offset and seek." Parquet adopts it for *queries*, with a footer that turns "find the rows from July" into "look up which row groups overlap July, fetch only those." Three formats, the same underlying move: don't make the reader walk the whole book to find one page. Put a small index somewhere cheap to reach, and let it point directly at the page you actually want.

You wrote a chapter. I want page 15. The index is what makes that request answerable without reading your whole book first.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [The Postman Reads the Envelope, Not the Letter](https://rinie.github.io/2026/06/04/postman-reads-envelope/) on Gutenberg 2.1 and page boundaries, [The Streaming Billy Is Not That Far Away](https://rinie.github.io/2026/07/11/streaming-billy/) on byte ranges and HTTP as a page-oriented protocol, and [Embracing the Web Instead of Fighting It](https://rinie.github.io/2026/07/13/embracing-the-web/) on XML's narrower, better-suited role inside containers built by others.*
