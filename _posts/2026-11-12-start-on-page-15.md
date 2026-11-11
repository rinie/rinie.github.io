---
layout: post
title: "Start on Page 15: Whitespace Only Counts Inside a Fence"
date: 2026-11-12
tags: [gutenberg-semantic, whitespace, fences, yaml, python, xml, json5, markdown, line-numbers]
level: technical
description: "The printer test from the UTF-8 post, applied to source: can a reader start on page 15 without reading page 1? XML namespaces, indentation and multi-line strings send them back. Whitespace should mean something only inside a visible fence, and even there each line should carry its own quotes, the way C joins adjacent string literals."
---

[The UTF-8 post](https://rinie.github.io/2026/08/29/one-lookup-per-book/) said a printer should be able to print pages 15 and 16 without the byte-order mark on page 1 or an error on page 82. The same test applies to anyone reading source: can they start on page 15? Two things send them back to page 1. One is meaning that was set earlier. The other is whitespace that carries meaning without showing. The rule that falls out is that whitespace counts only inside a fence, the fence has to be visible, and even inside it every line should carry its own.

---

## Where Page 1 Leaks In

XML leaks twice. A parser reports the whitespace between elements as text unless something tells it otherwise, so reformatting a file can change what it says. And a namespace prefix declared on the root element decides what a name on line 400 means, so line 400 can't be read on its own.

YAML and Python make the fence an indentation count, which is whitespace you can't see. To know which block line 40 sits in, you scan upward for the opener, and skimming stops working once the opener is off screen.

Multi-line strings leak the other way. In triple-quoted Python strings, JavaScript template literals and YAML block scalars, any line might be code or string content depending on a quote opened pages earlier, and the indentation inside the string is data.

---

## Fences That Show

JSON follows the rule. Whitespace outside strings is free, and inside a key or value it counts, with the quotes showing exactly where. Every key is a fence. JSON5 drops the quotes only where nothing can hide, because an identifier has no spaces to guard, and a key with a space in it still needs them.

Markdown makes the paragraph the unit and the blank line its fence. A newline inside a paragraph means nothing, so text can be wrapped at 80 columns and any paragraph can be read without the ones before it. It isn't airtight: two trailing spaces force a line break and four spaces of indentation start a code block, both whitespace that counts without showing.

C's adjacent string literals, and escapes like `\n`, carry the rule inside the string itself. Each line is a complete quoted fragment, so a line of `"First sentence.\n"` reads the same wherever you land. It costs quotes and escapes. Past a few lines the text wants to be a file, which is a page of its own, not a literal.

---

## The Rule

Whitespace carries meaning only inside a fence, and the fence has to show. [The white-line post](https://rinie.github.io/2026/07/27/dont-hide-the-fence/) asked for a seam that is findable without being obstructive, and this is that applied to blanks. No line should need an earlier one to be read either, which is [the test from the trailing-comma post](https://rinie.github.io/2026/11/11/the-last-item-is-not-special/): can you tell from one line, without its neighbors, where something starts and ends? The whitespace cases sort the same way as the commas. Quoted fragments, blank-line fences and one property per line pass, because the answer is on the line. Indentation blocks, namespace prefixes and multi-line strings fail, because the answer is on an earlier page. Start on page 15. If you can read it there, the format kept its promise.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [The Last Item Is Not Special](https://rinie.github.io/2026/11/11/the-last-item-is-not-special/) on one line carrying one meaning, [Don't Erase the White Line. Don't Pour Concrete Either.](https://rinie.github.io/2026/07/27/dont-hide-the-fence/) on seams that show, and [One DNS Lookup Per Book](https://rinie.github.io/2026/08/29/one-lookup-per-book/) on starting at page 15 without page 1.*
