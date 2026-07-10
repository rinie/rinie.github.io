---
layout: post
title: "Taste Comes Last: The Electrician Doesn't Care What Color Your Switch Is"
date: 2026-08-07
tags: [gutenberg-semantic, pace-layering, interface, construction, standards, resolver, sequencing]
level: general
description: "Watch a house or car get built and taste only enters at the end — the electrician and plumber finish without knowing what switch plate or faucet finish you'll pick. No committee designed the junction box dimension. It's the residue of a century of trades hitting the same friction until it stopped. No model, no architect — real-world use, arrived at purely from feedback, doing what a comprehensive standard designed from first principles usually can't."
---

Watch a house get built. The electrician runs wire, sets junction boxes, terminates circuits. The plumber runs pipe, sets valves, terminates supply lines. Both finish their work — genuinely finish, walk off site, get paid — before anyone has chosen what colour the switch plates will be or what finish the faucets will have. Taste enters last, after the functional work is already done, and the functional trades never had to wait for it or even ask.

This isn't [pace layering](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) in the sense the series has used it so far — it's not that switches change faster than wiring, though they do. It's something sharper: the *order* in which decisions get made, and the standard interface that makes deferring the aesthetic decision possible without blocking anyone.

---

## The Junction Box Is the Resolver

A standard electrical box is a fixed size, mounted at a fixed depth, with a fixed screw spacing for whatever cover plate eventually goes on it. The electrician doesn't need to know if the switch plate will be brushed nickel, matte black, or a smart dimmer with a companion app. The box's dimensions were standardised long before this particular house, by a committee the electrician has never met, for a reason that has nothing to do with this owner's taste: so that the *load-bearing decision* (where power gets delivered, how much current the circuit can carry, whether the wiring meets code) can be finished completely independent of the *aesthetic decision* (what the thing covering the box looks like).

The pipe thread does the same job for the plumber. NPT or BSP, whichever standard applies, has been fixed for over a century. The plumber terminates the supply line at a standard-threaded stub, and every faucet manufactured against that standard — regardless of finish, shape, or price point — will thread onto it. The plumber's work is complete and correct the moment the stub is set, regardless of which faucet the homeowner eventually picks, because the thread pitch is the actual interface and the faucet's appearance is not.

This is the same move [an interface makes over an implementation](https://rinie.github.io/2026/07/29/java-reversed-hierarchy-forgot-resolver/) in software, or [a REST API's contract](https://rinie.github.io/2026/07/28/at-sign-layer-boundary/) makes over whatever's actually running behind it, just built in copper and drywall instead of code. The junction box and the pipe thread are Gutenberg-layer standards — fixed, physical, agreed well in advance — that exist specifically so the Semantic layer riding on top of them, the owner's taste, can be decided at the very last possible moment without anyone downstream having to wait or redo anything.

---

## Sequencing Is the Actual Discipline

The pace-layering posts in this series have mostly asked "how fast does this change." This is a different question: "in what order do the decisions have to happen, and what has to be fixed first to make the later decisions free." A build schedule answers it explicitly. Structural work first, because everything depends on it and it's the most expensive to redo. Rough-in trades next — electrical, plumbing, HVAC — because they need to be inside the walls before the walls close, but their *termination points* are standardised, so the specific fixture is not yet a decision anyone needs to make. Finishes last — paint, fixtures, hardware — because they're cheap to change, don't affect anything structural, and genuinely are just taste.

The sequencing only works because the interface at each handoff is standard enough that the next trade — or the eventual owner — doesn't need information from further down the schedule to do their job correctly. The electrician doesn't need the paint colour to run wire. The plumber doesn't need the faucet brand to run pipe. Taste is deferred not because anyone decided taste doesn't matter, but because the interface was designed, generations ago, specifically to make deferring it free.

---

## When the Interface Isn't Standard, Taste Leaks Backward

The failure case is instructive. A custom, non-standard fixture — a faucet with a proprietary connector, a switch that needs a nonstandard box depth — breaks the sequencing entirely. Suddenly the plumber or electrician needs to know the specific aesthetic choice *before* finishing the rough-in, because the interface wasn't standard enough to absorb whatever gets decided later. The taste decision, which should have been free to make last, now has to be made early, under time pressure, before the trade that depends on it can even start. Everyone's schedule collapses toward the earliest decision instead of allowing the latest one.

Software has the identical failure mode, and it's worth naming directly: a backend built without a stable API contract forces the frontend team to know implementation details before they can build anything, and a UI change that wasn't anticipated by the interface forces a backend change to accommodate it. The interface's whole job is to make that unnecessary — to let the team doing structural work finish completely, and let the team doing taste work start whenever they're ready, neither one blocking the other. When the interface is genuinely standard, taste comes last for free. When it isn't, taste has to come first, expensively, for everyone.

---

## No Craft Can Block Another

There's a consequence of this sequencing worth naming directly: none of the trades can become another trade's bottleneck, because none of them are waiting on a decision that hasn't been made yet. The electrician doesn't sit idle waiting for the homeowner to pick a switch plate. The plumber doesn't sit idle waiting for a faucet decision. Every trade's schedule depends only on the fixed, undisputed standard beneath it — box dimensions, thread pitch, wire gauge, code requirements — never on a taste decision that might arrive late, might get revised twice, or might not get made until the week before move-in.

This is what actually buys the flexibility at the end of the process. Because the foundation — the standards, the interfaces, the load-bearing decisions — is settled and undisputed, the aesthetic layer on top of it can be genuinely adaptive: changed, delayed, argued about, revised, chosen at the literal last minute, without any of that indecision propagating backward and stalling a different trade's work. The flexibility isn't scattered evenly across the whole build. It's concentrated entirely at the end, on purpose, because everything underneath it was deliberately made non-negotiable first.

Invert the order and the flexibility disappears along with it. If the switch plate design had to be settled before the electrician could start, every trade downstream of that decision would inherit its uncertainty — the electrician's schedule would now depend on the homeowner's indecision, and so would everyone after the electrician. One undecided taste choice would bottleneck an entire crew. The standard interface is what prevents that dependency from ever forming: it lets the load-bearing work proceed on a foundation nobody's arguing about, precisely so the part people *do* argue about can stay flexible without costing anyone else their schedule.

---

## No Model, No Architect

Nobody designed the junction box. There was no committee that sat down with Brand's shearing layers or a whiteboard and decided, in advance, exactly where the load-bearing interface should sit between the electrician's work and the homeowner's taste. The standard box dimension is the residue of a century of electricians hitting the same friction — a box too small to work in safely, a depth that didn't match the wall assembly, a screw spacing that didn't fit the plates being manufactured — and the friction settling, trade by trade, job by job, wherever it stopped costing anyone money to keep arguing about it. Same for the pipe thread. Nobody theorised NPT into existence. It's what was left after enough plumbers over enough decades converged on one thread pitch because every other one kept causing leaks, mismatches, and callbacks.

This is worth naming plainly against the rest of this series' vocabulary: it's real-world use, arrived at with no model and no architect, purely from feedback accumulated over a timescale no single practitioner ever saw the whole of. Nobody who set the current standard could have predicted every faucet finish or switch style that would eventually be built against it — the standard didn't anticipate the future taste decisions, it just stopped being the thing anyone had to argue about, which is a different and humbler achievement than "designed correctly." The [engineering theorists building comprehensive standards from first principles](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/) — Ada's committee, the walled garden's real modularity — start from a model and hope the world matches it. The junction box started from a hundred years of people getting shocked, redoing work, and quietly agreeing on the version that stopped happening. No model outperformed no architect, because the trades doing the arguing were also the trades living with the consequences, every single day, for longer than any theorist would have had the patience to wait.

---

## The Faucet Doesn't Report Back to the Committee

One more thing the junction box and the pipe thread get right, worth noting because it's the same shape as [the meterkast's asymmetry](https://rinie.github.io/2026/08/03/the-meterkast-pattern/): the standards body that fixed the thread pitch a century ago has no idea what faucet you eventually bought, and doesn't need to. The interface is stable enough that an unbounded number of aesthetic choices can be made against it, forever, without any of them needing to report back or be anticipated in advance. That's what makes it a real interface rather than a negotiation — the electrician's box and the plumber's stub don't just defer the taste decision, they make the taste decision genuinely irrelevant to anyone whose job finished before it was made.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [The Meterkast Pattern](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) on standard interfaces at the public/private boundary, [com.google.gson Goes Nowhere](https://rinie.github.io/2026/07/29/java-reversed-hierarchy-forgot-resolver/) on modularity as replaceability, and [Cloud Breaks the Pace Layers](https://rinie.github.io/2026/08/04/cloud-breaks-the-pace-layers/) on what happens when the cost assumption underneath a pace layer changes.*
