---
layout: post
title: "More Than 10% Waste? That Was a Leap of Faith."
date: 2026-07-15
tags: [gutenberg-semantic, lean, waste, seams, architecture, breadth-first, craft]
level: general
description: "Waste is the measure the craftspeople use to validate the architect. Below 10%: the seams were correctly drawn, small adjustments, anticipated evolution. Above 10%: the architect made a leap of faith. The floor plan was drawn from theory, not from going to the Gemba. Small iterations keep the corrections small."
---

The architect draws the seams. The craftspeople build. The waste tells you whether the seams were right.

Not the feature velocity. Not the sprint completion rate. The waste — the rework, the seam revision, the integration cost that was not anticipated. The bricklayer who hits a pipe three times in the same wall. The DBA who keeps adding query hints around the same boundary. The developer who keeps working around the same API. All of them generating waste at the same seam. All of them telling the architect the same thing: this wall is in the wrong place.

The craftspeople cannot fake this signal. The waste is either there or it is not. The Lean measurement is honest in a way that sprint velocity is not.

---

## Waste Is the Use Signal for the Architect

The architect is the Def. The craftspeople are the Use. The waste is the feedback that validates or invalidates the architect's first pass.

The architect cannot validate their own seams. Proximity blindness. The floor plan looked correct from the desk. The architect designed the seam from theory — from what the domain should look like, from what the requirements said at the time, from what similar systems had done before. All of it reasonable. None of it as accurate as watching the bricklayers work.

The bricklayer who hits a pipe is not complaining. The bricklayer is reporting. The pipe is the Chesterton's Fence the floor plan did not show. The waste is the signal: the seam crossed a load-bearing boundary that was invisible at design time.

Lean named this. Waste is information. Not about the worker's capability — about the process design. The worker who generates waste at the same seam repeatedly is the ten-users-saying-it-sux signal for the architect. The sticky note next to the monitor. The workaround that became standard practice. The query hint that nobody removed because removing it breaks something.

Go to the Gemba. Watch where the waste accumulates. That is where the seam needs to move.

---

## The 10% Threshold

Not all waste is signal. Some waste is unavoidable — the learning curve, the tooling friction, the coordination overhead that exists even with perfectly drawn seams. The question is not whether there is waste. The question is how much, and where it concentrates.

**Below 10%:** the seams were approximately correct. Small adjustments. The wall moves six inches. The API gains one parameter. The buffer grows from 4K to 8K. Each correction is small, local, reversible. The craftspeople adjust and resume. The parallel progress continues. The critical path stays short. This is anticipated evolution — the architect knew the first pass was approximate and left room for the small corrections that practice always reveals.

**Above 10%:** the architect made a leap of faith. The seam was drawn from theory rather than from observation. The wall needs to move three metres. The API needs a complete redesign. The buffer strategy was wrong at the architectural level. The corrections are large, disruptive, and affect multiple crafts simultaneously. The parallel progress stops while the seams are renegotiated.

The 10% is the boundary between "the seams were approximately right" and "the seams were a guess." Both are normal. The difference is honesty about which one you were doing.

---

## Small Iterations Validate the Seams

The small iteration is the instrument that keeps corrections small. Not because small iterations produce better code — because small iterations reveal seam problems before they compound.

Each iteration is a breadth-first check on the seam positions. Not just "did we complete the feature" — "did the seams hold under actual use." The feature completion is the Semantic signal. The waste at the seams is the Gutenberg signal. Both matter. The lead engineer who measures only feature velocity is measuring only the Semantic layer and missing the Gutenberg feedback entirely.

The architect who insists on large iterations — "we need three months to build the full system before we can evaluate the seams" — is deferring the Lean validation until the corrections are expensive. The waste that would have been 5% after one month is 40% after three months, because three months of craftspeople building on wrong seams produces three months of rework when the seams are finally corrected.

Small iterations surface the waste early. The correction is still below 10%. The craftspeople adjust. The parallel progress resumes with corrected seams.

---

## The Lean Validation Loop

The complete model is the Lean loop applied to architectural seams:

1. **Architect draws seams** — breadth-first first pass, from Gemba knowledge not theory
2. **Craftspeople build** — parallel, each in their domain, each moving at craft pace
3. **Waste accumulates at wrong seams** — the honest Use signal
4. **Lead engineer measures waste** — below 10%: seams correct; above 10%: leap of faith
5. **Architect revises seams** — small correction: normal; large correction: honest rethink of the first pass methodology
6. **Craftspeople resume** — parallel, with corrected seams, velocity restored

The lead engineer is the resolver between the architect's Def and the craftspeople's Use. Not approving or rejecting the architect's vision — measuring the waste that reveals whether the vision was grounded or speculative.

The architect who welcomes waste measurement is the architect who trusts the process. The architect who resists waste measurement is the architect who made a leap of faith and does not want to know.

---

## The 10% Connection

This is the same 10% that appears in [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) — the portability overhead that buys the option to move to the next iceberg. Same number, two expressions of the same principle.

The 10% portability overhead says: accept a small known cost now to preserve the option to improve later. The 10% seam correction budget says: accept small seam adjustments as normal and expected, rather than treating any correction as a failure.

Both are the architect's honest acknowledgement of working at the boundary between what is known and what is not known yet. The 10% is not waste in the Lean sense — it is the investment in optionality that prevents the 40% correction later.

The architect who insists the first pass was perfect will pay more than 10% eventually. The domain always reveals what theory could not anticipate. The small iterations are the mechanism that keeps the revelation cheap.

**Below 10% waste: the seams were drawn from knowledge.**
**Above 10% waste: the seams were drawn from faith.**

The craftspeople know the difference. The waste tells them. The architect should listen.

Build, Measure, Learn beats the static plan. Not because the loop is faster — because the loop incorporates reality. The static plan incorporates theory about what the domain will look like before any iteration has revealed what it actually looks like.

Build: keep the seams clean, let the craftspeople work in parallel.
Measure: watch where the waste accumulates, go to the Gemba.
Learn: move the seam slightly, below 10%, resume.

The destination was never in the plan. The destination appeared through the loop. The architect who insists on the static plan mistakes the floor plan for the building. The building is what the craftspeople built. The floor plan is the hypothesis they tested.

Below 10% waste: the hypothesis was close. Above 10%: it was a leap of faith. Deming's PDCA — Plan, Do, Check, Act — is the full loop. In practice most organisations skip to Do and Check only validates the Do — "did we complete the work according to the plan." The schedule was followed. The story points were burned. The train left on time. But Check should validate both Do and Plan: not only "did we do according to plan" but "was the plan correct in the first place." Did the seams hold? Was the waste below 10%? Did the hypothesis survive contact with reality? The Check that only validates the Do produces a false positive when the plan was wrong — everything green on the dashboard, the waste accumulating unseen, the assumption that the Plan was correct never challenged. The Plan that skipped that half of the Check cannot learn.

Build, Measure, Learn. The loop corrects both.

The plan that learned.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Knowing Your Craft: Adding an Architect Slows Down, Adding a Bricklayer Speeds Up](https://rinie.github.io/2026/07/14/knowing-your-craft/) on craft-based shearing layers and the architect's correct role, [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) on the 10% portability overhead, [Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on watching where the waste accumulates, and [Breadth-First Is How You Find the Seams](https://rinie.github.io/2026/07/01/breadth-first-is-how-you-find-the-seams/) on the architect's first pass.*
