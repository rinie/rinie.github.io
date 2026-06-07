---
layout: post
title: "Knowing Your Craft: Adding an Architect Slows Down, Adding a Bricklayer Speeds Up"
date: 2026-07-14
tags: [gutenberg-semantic, architecture, craft, shearing-layers, bikeshed, linus]
level: general
description: "Architects do not scale. One architect sets the seams and steps back. Two architects fight about meaning while the craftspeople wait and the user pays for the delay. The bricklayers, carpenters, and electricians just build — they do not challenge the owner about the colour of the bikeshed. Linus does not Def-Push. He only says no, very loud, to waste."
---

One architect sets the seams and steps back. The craftspeople build. The user gets the feature.

Two architects fight about the seams. The craftspeople wait. The user pays for the delay.

The bricklayers do not fight about the colour of the bikeshed. The bikeshed is not a brick problem.

---

## The Bikeshed Is Always a Semantic Dispute

The bikeshed is always a Semantic dispute — colour is meaning, not structure. The bricklayer does not fight about the colour of the bikeshed because bricks are not colour. The electrician does not fight about cable runs and colour because they are in different domains. The carpenter does not fight about the paint choice because wood is not paint.

The architect fights about the bikeshed because the architect's domain is meaning — and colour is meaning. Two architects means two people whose domain is meaning, neither with the authority to end the dispute. The bikeshed gets painted, debated, repainted, reconsidered. The bricklayers wait.

The critical path runs through the Semantic dispute. Not because the dispute matters more than the bricks — because the bricklayers need the colour decision before they can tile the floor around the bikeshed base. One Semantic decision, held in dispute, blocks ten Gutenberg tasks.

This is the architect scaling problem. One architect: the seams are in one person's head, the colour is decided, the bricklayers build. Two architects: the seams are contested, the colour is debated, the user waits.

---

## Craft-Based Shearing Layers

Stewart Brand's shearing layers are temporal — each layer of a building moves at a different pace over time. Site slow, Stuff fast, the layers shearing against each other at the boundaries.

There is a second axis: craft-based shearing layers. Teams and disciplines that can move independently when the seams between them are clean — not because one moves faster than the other over time, but because clean seams mean neither has to wait for the other at all.

**HTML, CSS, JavaScript, HTTP** — four crafts, four domains, four paces:

- HTML: the content author's domain. Moves at writing pace.
- CSS: the designer's domain. Moves at design pace.
- JavaScript: the developer's domain. Moves at feature pace.
- HTTP: the infrastructure team's domain. Moves at deployment pace.

When the seams are clean — when HTML does not carry inline styles, when JavaScript does not generate HTML strings, when CSS does not depend on JavaScript class names — each craft moves independently. The designer changes the CSS without waiting for the developer. The developer ships JavaScript without waiting for the designer. The infrastructure team upgrades HTTP/2 without touching anyone's code.

When the seams are muddy — CSS-in-JS, server-side rendering that fuses all four, frameworks that own the whole stack — the critical path runs through every craft simultaneously. Nobody moves until everyone is ready. The architect who owns all four layers is on every critical path at once.

**SQL as the independent craft seam:**

SQL kept separate from the application is the clean seam that keeps the data craft off the critical path of every other craft. The DBA optimises the query without touching the application code. The developer adds a feature without understanding the execution plan. The seam between them is clean. Both move at their own pace.

The ORM that removes the SQL seam also removes the independence. Now the developer is making query decisions without the DBA's domain knowledge. The DBA has no clear domain. The critical path runs through both crafts simultaneously. One more Leonardo bottleneck — the person who has to touch everything before anything can move.

---

## The Correct Architect Role

The architect's job is the breadth-first first pass:

1. Find the seams
2. Mark the walls
3. Assign the craft domains
4. Step back

The craftspeople build their Gutenberg parts without needing the architect after that. The bricklayer knows their walls. The carpenter knows their frames. The electrician knows their cable runs. Each domain is clear. Each craft moves independently. The architect's value was in the first pass — not in attending every daily standup, not in approving every colour choice, not in staying on the critical path of every decision.

The architect who stays on the critical path after the seams are marked did not draw the seams clearly enough. The floor plan needs revision, not more architects.

The architect who adds a second architect is admitting the first architect could not finish the first pass. Two architects will redraw the floor plan simultaneously, disagree on where the walls are, and bill the user for both rewrites.

---

## Linus: Not Def-Push. Only No to Waste.

Linus Torvalds does not scale to two architects. He does not need to. His role is not designing features — it is defending the seams against waste.

He does not tell people what to build. He does not impose his vision on the kernel's direction. He does not add features because he personally wants them. The kernel's direction is Use-Pull — driven by the hardware it needs to support, the workloads it needs to run, the security requirements that reality imposes. Linus maintains the seams while that direction evolves.

What he says no to — loudly, precisely, with explanation — is waste:

- Code unnecessarily complex: no
- Abstractions that add layers without adding value: no
- APIs that break userspace: no, loudly, permanently
- C++ in the kernel: no, with a famous explanation
- Patches solving the wrong problem at the wrong layer: no

Each rejection is a seam defence. Not "I prefer it differently." Precisely: "this crosses the waterline in the wrong direction, this adds Semantic complexity to the Gutenberg layer, this moves a load-bearing seam without understanding why it is load-bearing."

The loudness is signal strength, not ego. The kernel receives thousands of patches. A quiet no gets lost. A loud no with precise diagnosis is the diff that teaches the contributor where the seam actually is. The next patch from the same contributor is better — because the loud rejection was also a precise diagnosis of the layer violation.

The bricklayers do not need Linus to pick the colour. They need Linus to tell them which walls are load-bearing. He tells them. Loudly when it matters. The craftspeople build. The user gets the kernel.

---

## No Architect and Stable Seams

The fourth model — no architect, stable seams — also works. Unix. The internet. Git. The web.

Berners-Lee stepped back. The web outgrew its architect by arithmetic — too many people, too many domains, too many crafts for one person's head to hold the seams. The seams he drew (HTML structure, HTTP transport, URL identity) were clean enough to survive without him. The craftspeople kept building. The 10x arrived that nobody planned.

Linus will step back eventually. The kernel's seams are clean enough that they will survive. The ABI is documented. The driver interface is stable. The load-bearing walls are known. The craftspeople will keep building because the seams are the stable guide, not the architect.

Two hoorays not three. One architect does the first pass and steps back. Two architects fight until someone steps back anyway — but later, at the user's expense.

---

## The User Pays

The user pays for every architectural dispute that lands on the critical path.

The three-hour review that produces no decision: the user pays in delayed features. The CSS-in-JS framework that requires a developer and a designer to coordinate every style change: the user pays in slower iteration. The ORM that removed the SQL seam: the user pays in the performance regression that needed a DBA to diagnose and a developer to fix simultaneously.

The user is never in the room when the architects fight. The bikeshed is painted in a room the user has never visited. The user just sees: the feature that was promised last quarter still is not here.

The bricklayer does not fight about the colour of the bikeshed. The bricklayer builds the bikeshed and asks: where do you want the door? One question. One answer. One craft domain. The bikeshed is done.

The bricklayer's legitimate bikeshed is brick quality — not paint colour. "These bricks are underfired, they will not hold load" is a Gutenberg observation about Gutenberg quality. The architect should listen. The bricklayer knows bricks. "I think the living room should be painted blue" has crossed the waterline. Colour is above the bricklayer's domain. Not because the bricklayer is wrong about blue — because colour is not a brick.

Every craftsperson has legitimate opinions within their domain and illegitimate opinions outside it. The DBA who says "this query will full-scan 50 million rows, it needs an index" is doing their job. The DBA who says "I think the application should be redesigned around a graph model" has crossed the waterline. The developer who says "this API response is 40MB, the client will time out" is doing their job. The developer who says "I think the whole product should pivot to B2B" has crossed the waterline.

The seam between Gutenberg and Semantic is also the seam between craft expertise and architectural opinion. Below the waterline: the craftsperson's domain, their legitimate bikeshed, their expert knowledge of the material. Above it: the architect's domain, meaning, purpose, colour. The craftsperson who stays below the waterline adds speed. The craftsperson who crosses it becomes another architect — and two architects fight.

Two architects fight about the door. The user pays.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Working on the Same Page](https://rinie.github.io/2026/06/06/working-on-the-same-page/) on HTML/CSS/JS as clean craft seams, [After 25 Years SQL Still Wins](https://rinie.github.io/2026/06/11/sql-still-wins/) on SQL as the independent craft seam, [Breadth-First Is How You Find the Seams](https://rinie.github.io/2026/07/01/breadth-first-is-how-you-find-the-seams/) on the architect's first pass, and [Nothing Is Confusing to Me: The Inmates Are Running the Asylum](https://rinie.github.io/2026/06/20/nothing-is-confusing-to-me/) on proximity blindness — the architect who stopped going to the Gemba.*
