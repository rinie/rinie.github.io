---
layout: post
title: "They Bought a Porsche and Had No Money Left for Gas"
date: 2026-08-24
tags: [gutenberg-semantic, digital-government, germany, architecture-astronauts, dokufis, resolver, worse-is-better]
level: general
description: "Friedrichshafen's T-City spent millions on visible smart-city showpieces starting in 2007 and left the town hall's back-office paperwork untouched. A school principal's own verdict: they'd bought a Porsche and had no money left to fill it with gas. A rural German district on the Dutch border did the opposite — digitised the boring internal workflows first, in 2013, and only then built anything a citizen would see."
---

Germany's 2026 Digital Decade scorecard from the EU Commission puts the country at 78.1 out of 100 for digital citizen services, nearly seven points under the EU average of 84.6, and worse still on electronic identity — only 15% of the population uses the eID function on their national ID card, fourth-worst in the EU. That's the familiar story. The less familiar one, reported recently by [Heise's Missing Link](https://www.heise.de/hintergrund/Missing-Link-Digitalisierungswueste-Was-die-Grafschaft-Bentheim-besser-macht-11403797.html), is what a rural district of 145,000 people on the Dutch border did instead of accepting it as fate.

---

## The Porsche With No Money Left for Gas

Starting in 2007, Deutsche Telekom and the town of Friedrichshafen spent millions turning the Lake Constance community into a flagship "T-City" — smart electricity meters, mobile ticketing, telemedicine. When the results came in, a school principal delivered the verdict that stuck: they'd *bought a Porsche and had no money left to fill it with gas.*

The failure was sequencing. Friedrichshafen built the visible layer — the smart-city showpieces citizens would actually see — while the internal workflows inside the town hall stayed exactly as paper-bound as they'd always been. That's [the Architecture Astronaut mistake](https://rinie.github.io/2026/08/12/architecture-astronauts/) at municipal scale: an impressive demo, comprehensive in its own domain, solving the visible template of "smart city" while the actual bottleneck — how a form moves from a citizen's screen to a caseworker's decision — was never touched.

The article names the more common version of this failure directly: a citizen submits a form digitally through the portal, and at the office it gets *printed out, stapled into a cardboard folder, and routed through internal mail.* The Semantic-layer front door is real. The Gutenberg-layer process behind it never changed.

---

## Inside-Out, Starting With the Boring Part

Grafschaft Bentheim's district administration didn't wait for a federal ministry to hand down a finished template. In 2013 — while, in the district's own account, many other authorities still confused digitisation with buying faster photocopiers — it started with the job centre's case files, digitising the welfare casework record first. Not a citizen-facing feature. Paperwork nobody outside the building would ever see.

The district's own principle, stated as their working maxim: *digitisation from the inside out.* Digital case coordinator Daniel Keuter's reasoning is worth keeping in his own terms — analogue processes can't just be carried over one-to-one into digital form; they have to be redesigned for it. Building a working digital foundation in the background came first. A citizen submitting a form digitally means nothing if the process behind it still ends in a printed page.

The results, a decade later, are concrete rather than aspirational: 3,500 welfare cases now managed fully digitally end to end, 400 black plastic filing trays disposed of because they'd become unnecessary, no more staff pushing file carts down hallways. The district's Chief Digital Officer used to have roughly 100 physical invoice folders stacked on his desk every week for signature. Now the entire approval runs as a digital workflow he can sign off from anywhere.

---

## A Standard Built to Prevent Its Own Lock-In

The software the district rents, rather than owns outright, comes from a vendor called d.velop — but the more interesting detail is what that vendor did *with competitors*. d.velop worked with other document-management vendors, through an industry association, to build a shared interchange standard called DokuFIS, specifically so that documents, metadata, and file information can be exported and reprocessed in a fully standardised way. The vendor's own stated commitment: data isn't encrypted in proprietary formats, every relevant interface is publicly documented, and the municipality remains the owner of its own data at all times.

That's [the same commodity-over-smart-wire discipline already argued through S3 and HDFS](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/), showing up in German municipal document management instead of cloud object storage — a group of competing vendors choosing to keep the interchange format dumb and portable, on purpose, rather than each building a smart, proprietary format that would quietly become a lock-in point once it had enough customers trapped inside it.

The district's posture toward centrally mandated solutions follows the same logic without waiting passively for them. When federal digitisation law was announced, Bentheim didn't sit idle for a state-level blueprint — it built its own access points. But it didn't isolate itself either: where a state-developed shared service already exists and works — parental allowance applications, for instance — the district plugs directly into it rather than reinventing something that's already solved. Build locally where the gap actually is. Reuse centrally where a working resolver already exists. Neither pure not-invented-here, nor waiting on Godot.

---

## Foundation, Then Facade

The district's own summary of the lesson, offered directly to other municipalities: *the key is the courage to set priorities — build the back-end foundation first, then design the facade; clean up the processes first, then deploy the AI.* That's close to a perfect restatement of this series' own argument, arrived at independently by people running a county government rather than reading about resolvers. Keuter's closing advice to other administrations still stuck waiting for perfect conditions: *"Just start."*

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Architecture Astronauts](https://rinie.github.io/2026/08/12/architecture-astronauts/) on solving the template instead of the actual problem, [S3 Out-Evolved HDFS by Refusing to Be Smart](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/) on commodity interchange formats over proprietary lock-in, and [Worse Ages Better Than Perfect](https://rinie.github.io/2026/06/28/worse-ages-better-than-perfect/) on pragmatic local solutions that ship versus comprehensive ones that don't.*

Source: [Missing Link: Digitalisierungswüste? Was die Grafschaft Bentheim besser macht](https://www.heise.de/hintergrund/Missing-Link-Digitalisierungswueste-Was-die-Grafschaft-Bentheim-besser-macht-11403797.html), heise online, August 2026.
