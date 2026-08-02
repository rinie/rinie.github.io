---
layout: post
title: "Aggregation Theory: Modularize the Supplier, Integrate the Customer"
date: 2026-08-11
tags: [gutenberg-semantic, aggregation-theory, ben-thompson, resolver, google, amazon, netflix, uber, airbnb]
level: general
description: "Ben Thompson named the pattern in 2015: the winning move on the internet is to modularize the supplier side and integrate the customer relationship. That's the aggregator's whole business model in one sentence — and it's also the clearest possible definition of what a resolver actually does at platform scale."
---

Ben Thompson named a pattern in 2015 that this series keeps rediscovering from different directions: the winning move for an internet business is to modularize one side of the value chain and integrate the other. Specifically — [modularize the suppliers, integrate the customer relationship](https://stratechery.com/2015/aggregation-theory/). He called it Aggregation Theory, and it's worth reading through this series' own vocabulary, because it turns out to be the clearest possible definition of what a resolver does once it operates at platform scale.

---

## Before the Internet, Distribution Was the Moat

Thompson's framing of the old world: a value chain has suppliers, distributors, and consumers. Before the internet, the way to make outsize profits was owning distribution — and once you owned distribution, you integrated backward into supply, because there were always more consumers than suppliers, and controlling the scarce relationship gave you leverage. Newspapers integrated backward into content creation. Book publishers integrated backward into controlling authors. Hotels integrated brand trust with vacant rooms. In every case, the distributor decided which suppliers got access to consumers — pure Def-Push, enforced by the physical cost of reaching an audience at all.

The internet broke this in two specific ways, in Thompson's own terms: distribution of digital goods became free, and transaction costs collapsed to zero. Once both of those things were true, a new company didn't need to own supply anymore. It needed to own the customer relationship and let supply become interchangeable underneath it.

---

## The Aggregator Is a Resolver With a Business Model

Read Thompson's examples through [this series' vocabulary](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) and the pattern is unmistakable. Google modularised individual web pages — any page, from any publisher, became directly reachable, the Gutenberg-layer content commoditised and interchangeable. What Google integrated instead was the *semantic* layer: search intent, click behaviour, and advertising, tied to a single, exclusive relationship with the user asking the question. Amazon modularised book distribution — any publisher's book, sold the same way — while integrating payment information and the customer relationship. Netflix modularised broadcast availability — the entire library, available in any order, at any time — while integrating subscriber billing and viewing history.

In every one of Thompson's examples, the supplier side is where things get commoditised, interchangeable, resolved fresh on demand — the Gutenberg layer, stripped of any semantic weight of its own. The customer relationship is where the aggregator concentrates ownership — the Semantic layer, the one thing kept scarce and exclusive on purpose. That's not a coincidence of platform economics. It's the resolver pattern this series keeps finding everywhere else — DNS resolving a name to whichever server currently holds it, a CDN resolving a request to whichever edge node is nearest — except deployed as a business model instead of a protocol. [The mall doesn't come to you, you go to the mall](https://rinie.github.io/2026/08/03/the-meterkast-pattern/); the aggregator's whole value proposition is being the one place users go, while what's actually on the shelves underneath stays interchangeable.

---

## The Virtuous Cycle Is a Resolver Getting Better With Use

Thompson's mechanism for why aggregators win, not just exist, matters too: more users attract more suppliers, which improves the experience for users, which attracts more users. That's the exact shape of [a resolver that learns from repeated use](https://rinie.github.io/2026/08/08/use-specialisation/) rather than staying static — except here the thing being learned isn't one consumer's taste, it's the whole aggregate demand curve, and the improvement compounds because every additional user makes the resolver more valuable to the next supplier deciding whether to show up.

This is also exactly why an aggregator, once it wins, becomes [a Def-Push resolver worth exploiting](https://rinie.github.io/2026/08/09/playlist-resolver-take-for-a-ride/). The suppliers were modularised specifically so the aggregator, not any individual supplier, controls the terms of access to the customer relationship it now owns exclusively. Google modularised publishers and then became the toll booth publishers depend on for traffic. Amazon modularised booksellers and then became the toll booth publishers depend on for shelf space. The aggregation move and the eventual squeeze aren't two different stories. They're the same mechanism, read at two different points in its lifecycle.

---

## Where the Framework Runs Out

Aggregation Theory explains the win condition better than almost anything else in this series' reading list, but it has a boundary worth naming: it describes markets where transaction costs were the thing holding the old distributor's moat together. It has less to say about the cases this series keeps returning to where the moat was never really transaction cost at all — a phone number, an OBD port, a pipe thread. Those didn't get disrupted by an aggregator, because there was never a proxy relationship to modularise in the first place. Nobody aggregates number portability. There's nothing to integrate, because nobody was ever positioned to extract rent on the resolver to begin with.

That's the cleanest test Aggregation Theory offers this series in return: if you can name the aggregator that would eventually own the customer relationship, you're looking at a Def-Push resolver waiting to be built or replaced. If you can't — if there's no proxy being modularised, no scarce relationship to integrate — you're probably looking at the rarer thing this series keeps circling back to: a resolver nobody profits from owning, which is the only kind that doesn't eventually take you for a ride.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Every Def-Push Playlist Resolver Gets Replaced](https://rinie.github.io/2026/08/09/playlist-resolver-take-for-a-ride/) on aggregators eventually taking their users for a ride, [Use-Specialisation](https://rinie.github.io/2026/08/08/use-specialisation/) on resolvers that improve with repeated use, and [The Meterkast Pattern](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) on the resolver nobody profits from owning.*

Source: [Aggregation Theory](https://stratechery.com/2015/aggregation-theory/), Ben Thompson, Stratechery, July 21, 2015.
