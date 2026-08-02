---
layout: post
title: "We Hate Air: IKEA's Actual Design Philosophy, and Where the Billy Bookcase Came From"
date: 2026-08-14
tags: [gutenberg-semantic, ikea, flatpack, data-layout, sql, standards, disposability, taste-comes-last]
level: technical
description: "IKEA's CEO says it plainly: we hate air. Shipping empty space is expensive, so furniture ships disassembled. The same principle applies to data — every unfetched column you pull anyway is air, shipped at a cost, paid every single time. IKEA also explains why to stick to screws when you can't bundle an allen key, and why disposable, ugly SQL beats elegant, precious alternatives."
---

The Billy bookcase has been doing quiet work throughout this series — the indifferent shelf, [the flatpack house Android assembles from shared parts](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/), the junction box nobody had to design twice. That imagery traces back to a real source worth crediting directly: Taylor Troesh's [IKEA-Oriented Development](https://taylor.town/ikea-oriented-development), which takes IKEA's actual design philosophy — not the metaphor, the real business decisions — and reads it as a discipline for building software. It's worth doing properly, because IKEA turns out to have already solved three problems this series keeps circling.

---

## We Hate Air

IKEA's CEO states the company's actual founding constraint in four words: *"we hate air."* Furniture ships disassembled specifically because empty space is expensive to move — when IKEA started selling the Ektorp sofa disassembled in 2010, they cut package size in half and removed 7,477 trucks a year from the road. Not a design aesthetic. A shipping-cost calculation, ruthlessly applied.

Troesh's translation to software is exact, and sharper than this series has put it before: *"packaging is the product; data layouts matter."* Take his own example — three versions of the same query, fetching a user's points total for a country:

```js
// ships every row, every column, then sums in application code
const usrs = await sql`select * from usr where country = 'JP'`;
let points = 0;
for (const usr of usrs) points += usr.points;
```

```js
// ships every row's full width, then a second query for the sum
const usrs = await sql`select * from usr where country = 'JP'`;
const [{ points }] = await sql`select sum(points) from usr where id in ${...}`;
```

```js
// ships nothing but the answer
const [{ points }] = await sql`select sum(points) from usr where country = 'JP'`;
```

Every row and column fetched but not needed is air — shipped anyway, at a real cost in bytes crossing a network, paid every single time the query runs. This is [the Use-Specialisation argument](https://rinie.github.io/2026/08/08/use-specialisation/) from the other direction: staying wide until the point of use was about not breaking when the schema grows. IKEA's version is about not paying, in bandwidth and latency, for width nobody asked for in the first place. Both arguments point at the same query. They're not the same argument — one is about correctness, one is about cost — and a system that gets the first right can still be shipping enormous amounts of air.

---

## Stick to Screws, Not Allen Keys You Can't Bundle

IKEA furniture needs exactly two tools, and one of them — the allen key — comes in the box every time, because IKEA can't assume you own one. Everything else is designed around the screwdriver, the tool virtually everyone already has.

Troesh's software translation names the discipline precisely: *"in the computing world, screws are made of plaintext, HTTP, etc. Today's shells and standard libraries offer ubiquitous screwdrivers like regex manipulation, HTTP processing, and JSON parsing. If you can't bundle allen keys for your hex fasteners, stick to screws."* This is [the junction box argument](https://rinie.github.io/2026/08/07/taste-comes-last/) stated as a design rule rather than an observation: don't require a proprietary tool unless you're prepared to ship it yourself, every time, to every consumer. A web API that only works with a bespoke SDK is an allen-key fastener with no allen key in the box. A web API reachable with `curl` and readable as JSON is a screw — ordinary, universal, assumable.

Troesh's closing jab lands the same point about longevity that [this series has made about Wirth's Law](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/): *"my Mario Kart 64 cartridge probably won't inform me that Python 2.7 was deprecated. If your program isn't designed to work 20 years from now, it won't."* A cartridge built on the console's universal screw — the physical interface every N64 has — outlives a program built on a dependency someone will eventually stop maintaining.

---

## Composable and Disposable

IKEA furniture isn't built to last, and Troesh's quoted line names this as the actual feature, not an accident: *"flimsiness is part of its appeal. Because when the door of a cabinet starts to sag off plumb, that means you get to buy another one."* Disposability isn't a defect to route around. It's what makes the furniture safe to experiment on — hackable, in the Kallax-hack sense, because nobody's precious about a shelf that costs less than dinner.

Troesh extends this straight into SQL, and it's a genuinely sharp point this series hasn't made yet: *"SQL is ugly, but there's a good reason it's the lingua franca of tech. People embrace SQL's blemishes because it's generally fast and queries are disposable."* Nobody guards a SQL query as precious architecture. You write it, run it, throw it away, write the next one, and occasionally dig an old one out of your own history because it's worth recycling. That disposability is exactly [what makes Pets vs. Cattle work as an infrastructure philosophy](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) — cattle get replaced without ceremony, precisely because nobody named them.

Troesh's three rules for making software hackable are worth keeping as a closing checklist: make experimentation effortless — nobody edits code that takes forty seconds to recompile. Embrace reliable mainstream formats — CSV, JSON, RSS, webhooks — the software equivalent of the screwdriver everyone already owns. And write code that can be replaced — clear inputs and outputs, disposable detail in between. *"We intuitively call irreplaceable code 'complicated' or 'spaghetti.'"* Code that can't be thrown away and rebuilt isn't precious. It's a liability wearing precious as a disguise.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Use-Specialisation](https://rinie.github.io/2026/08/08/use-specialisation/) on staying wide until the point of use, [Taste Comes Last](https://rinie.github.io/2026/08/07/taste-comes-last/) on the standard interface that lets specialisation happen safely, and [Wirth's Law Only Holds If You Recompile the Whole Iceberg](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/) on the flatpack house this whole thread traces back to.*

Source: [IKEA-Oriented Development](https://taylor.town/ikea-oriented-development), Taylor Troesh, taylor.town, June 14, 2023.
