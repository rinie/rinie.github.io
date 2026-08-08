---
layout: post
title: "Neither the Tree Nor the Deluge: Why Ten Results and a 'Next Page' Button Won"
date: 2026-08-22
tags: [gutenberg-semantic, search, yahoo, altavista, pagerank, google, use-specialisation, pagination]
level: general
description: "Before Google, search had two failure modes on offer: Yahoo's exposed category tree, or AltaVista's 40,000 undifferentiated results. Google's actual interface innovation wasn't PageRank alone — it was refusing both. Ten ranked results, a next-page button if you needed more. Stay wide underneath, narrow hard at the point of use, let the user pull for anything beyond that."
---

A recent [Ars Technica retrospective on pre-Google search](https://arstechnica.com/gadgets/2026/08/remembering-the-pre-google-web-when-search-was-an-experiment/) lays out three genuinely different answers to the same question — how do you help someone find something on a web too big to browse — and the interesting part isn't which one won commercially. It's that two of the three failed for opposite reasons, and Google's actual interface innovation was refusing both failure modes at once.

---

## The Tree: Yahoo's Directory

Yahoo's original model was a human-edited hierarchy — *"organized into nested subject categories and maintained by people rather than fully automated ranking systems."* Getting listed meant justifying your site's significance to an actual editor; the piece recounts a regional cancer-society chapter rejected as *"not significant enough"* for the main directory.

This is [the same mistake already named in Matter's cluster tree and Zigbee2MQTT's network graph](https://rinie.github.io/2026/08/21/waterline-identity-is-a-label-too/) — the producer's own organisational scheme, exposed directly as the thing the consumer has to navigate. A category tree is genuinely useful to the people maintaining it. It's a resolver failure the moment a stranger has to understand *your* taxonomy correctly, on the first try, to find anything — and, exactly as the article notes, human curation *"couldn't scale forever."* The tree wasn't wrong. It was a Gutenberg-layer artifact of how the directory was built, surfaced as if it were the Semantic-layer answer to "where do I find this."

---

## The Deluge: AltaVista's 40,000 Results

The opposite failure showed up in the crawler-based engines. AltaVista indexed fast and broad — genuinely impressive for 1996 — but a query could return, in one interviewee's account, *"40,000 results that would leave you just as confused as shouting into a crowded room."*

This isn't the tree problem. Nothing is being organised according to someone else's internal structure — there's no structure being imposed at all. It's the opposite failure: a wide pipe with no resolver doing any narrowing whatsoever. All Gutenberg, no Semantic layer riding on top of it. [Use-specialisation argues for staying wide until the point of use](https://rinie.github.io/2026/08/08/use-specialisation/) — but staying wide is only half the discipline. The other half is narrowing hard, and confidently, exactly at the point where a human actually needs an answer. AltaVista kept its promise on the first half and never delivered the second.

---

## Neither: Ten Results, Ranked, With a Next-Page Button

Google's actual interface decision, once PageRank gave it a real ranking signal, was to reject both models. Not a category tree to navigate. Not a wall of forty thousand undifferentiated links. Ten results, ranked by a genuine trust signal — *"treated links as signals of authority"* — with everything else available exactly one click away behind a page number, never forced on the user up front.

That's Use-Specialisation, stated as a product decision rather than a coding pattern. The full index stays wide, underneath, untouched — every page AltaVista would have shown you is still in there. What surfaces by default is narrowed hard, to the ten the ranking signal trusts most. And if the ten don't answer the question, the Use side pulls more, one page at a time, on its own initiative rather than being buried in the deluge unasked. The article's own framing captures the same point without quite naming it: Google's *"stripped-down interface"* mattered as much as the ranking underneath it — *"search should be a utility, not a portal amusement park."* A utility narrows to what you need and gets out of the way. A portal, tree or deluge alike, hands you its own internal shape and calls that help.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Below the Waterline, Identity Is a Label Too](https://rinie.github.io/2026/08/21/waterline-identity-is-a-label-too/) on the producer's structure leaking into the consumer's API, [Use-Specialisation](https://rinie.github.io/2026/08/08/use-specialisation/) on staying wide until the point of use, and [Every Def-Push Playlist Resolver Gets Replaced](https://rinie.github.io/2026/08/09/playlist-resolver-take-for-a-ride/) on the business story behind this same transition.*

Source: [Remembering the pre-Google web, when search was an experiment](https://arstechnica.com/gadgets/2026/08/remembering-the-pre-google-web-when-search-was-an-experiment/), Alan Bradley, Ars Technica, August 7, 2026.
