---
layout: post
title: "The Diamond Shows Up Eventually: Network, Relational, and Object Databases"
date: 2026-08-25
tags: [gutenberg-semantic, databases, codd, relational-model, oodbms, resolver, hourglass]
level: technical
description: "A network database stores data as physical pointer chains — a building where the only way from Accounting to Payroll is a specific elevator shaft installed when the building went up. Codd's relational model wasn't 'tables are nice.' It was isolating the Gutenberg/Semantic boundary in exactly one place instead of smearing it across every application. Object databases repeated the network mistake decades later, with better marketing."
---

A network database — CODASYL, IMS-era hierarchical — stores data as physical pointer chains. Sets link one record to another, wired in at schema-design time. To answer a question you don't query, you *navigate*: walk from Customer to Order to LineItem exactly along the pointers someone installed. It's a building where the only way from Accounting to Payroll is a specific elevator shaft installed when the building went up. A new question with no shaft to it means rewiring the building, not asking.

---

## The Hourglass Arrives

The relational model decoupled what you're asking from how it's stored. Rows aren't connected by baked-in pointers — they're facts satisfying a predicate. A join isn't a pre-wired path, it's computed on demand by matching values, a postal address system instead of hardwired shafts. Any point reaches any other point through the same uniform mechanism, because location got abstracted into something addressable rather than something physically tethered.

In [the framework this series keeps returning to](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/): network models are the diamond. Fast-changing, use-specific navigation logic gets fused directly into the physical layer, so every new access pattern touches both ends at once. The relational model is the hourglass — relational algebra is the stable, meaning-bearing waist, physical storage sits below and can be re-engineered freely (B-trees, heaps, column stores), arbitrary query shapes sit above and can be invented freely, because the waist never moves. Codd's insight wasn't "tables are nice." It was isolating the Gutenberg/Semantic boundary in exactly one place — the query optimizer — instead of smearing it across every application that touches the data.

---

## The Same Mistake, Better Marketing

Object databases tried to kill what got called the "impedance mismatch" between application objects and relational rows by storing objects with direct in-memory-style references to other objects. That's the network model again — pointer-chasing at write time instead of asking declarative questions the system can optimise. Same coupling, new syntax, decades later, sold as innovation rather than as a return to the elevator-shaft building.

That's also why relational survived the "relational is dead" wave that NoSQL and object databases both rode in on, and why OODBMS mostly didn't: [the hourglass out-evolves the diamond](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/), for the identical reason S3 out-evolved HDFS a decade later. A stable, narrow waist that both sides can be redesigned around beats a fused representation every time the fusion has to survive contact with a question nobody anticipated when the schema was drawn.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [S3 Out-Evolved HDFS by Refusing to Be Smart](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/) on the same hourglass-beats-diamond pattern at the storage layer, and [Use-Specialisation](https://rinie.github.io/2026/08/08/use-specialisation/) on keeping the waist narrow and the pipe wide on either side of it.*
