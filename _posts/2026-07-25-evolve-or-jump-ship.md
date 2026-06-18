---
layout: post
title: "Evolve or Jump Ship? Frameworks Come With an Iceberg Attached"
date: 2026-07-25
tags: [gutenberg-semantic, frameworks, libraries, lock-in, composition, seams]
level: general
description: "A library is a tool in your hold — you call it, you can swap it. A framework calls you, and it comes with an iceberg: the file structure, the routing, the build, the deployment, the whole mental model. The question before you board isn't whether it's a good iceberg. It's whether you can leave when you need to."
---

There's a one-line test for telling a library apart from a framework, and it has nothing to do with size. A library is something you call. A framework is something that calls you.

When you use a library, you write the structure and you reach into the library for a specific job — parse this date, sort this list, fetch this URL. The library is a tool sitting in your hold. If you want a different one, you change where you call it and you're done; the shape of your program doesn't move. When you use a framework, the framework writes the structure and your code fits into the slots it provides. You are a module on the framework's shelf, arranged the framework's way.

That difference sounds academic until you try to leave. And whether you can leave is the only question about a framework that actually matters in the long run.

---

## Frameworks Come With an Iceberg

When you adopt a framework, you are not adopting a tool. You are boarding an iceberg, and the iceberg includes far more than the part you evaluated.

You get the file structure convention. The routing model. The way state is managed. The build system. The deployment model. The testing setup. And underneath all of those visible pieces, the mental model that ties them together — the assumptions the framework makes about how an application is shaped, which now become assumptions your code makes too.

None of that is technical debt in the usual sense. It's commitment. You boarded this particular iceberg, and the cost of moving to a different one isn't paid piece by piece — it's paid all at once, because every piece is entangled with every other piece. Change the framework and the file structure changes, the routing changes, the build changes, the tests change, all in the same breath.

---

## Hotel California

The trap has a familiar shape: you can check out any time you like, but you can never leave.

Look at a large enterprise application five years into its life on a single framework. The components are the framework's components. The services are the framework's services. The routing, the forms, the HTTP calls, the tests, the deployment — all of them are written in the framework's idiom, because that's what using the framework meant. The exit is clearly drawn in the architecture diagram. It is nowhere to be found in the actual code, because there is no longer any part of the code that doesn't assume the framework underneath it.

That's not a failure of discipline. It's the default outcome of boarding an iceberg and never planning for the day you'd need to get off it.

---

## Three Ways Off

When the day comes — and for most frameworks it does — there are three ways it can go.

**Evolve.** The best case. The framework is actively changing, and it gives you a path from the old way to the new way without a full rewrite. React moving from class components to hooks, Vue moving to its newer composition style — the hotel renovated, the rooms got better, and you could move room by room rather than all at once. You stay on the iceberg because the iceberg is still going somewhere good.

**Jump ship.** The framework has stopped moving. The activity has dwindled, the security patches come slower, the community has drifted elsewhere. The iceberg is melting, and evolving along with it just means treading water. The right move is to leave before it's gone — while you still can do it on your own schedule.

**Forced jump ship.** The worst case, and the one that burns people. The framework's next major version is so different it's effectively a different framework wearing the same name, and there's no real path from the old to the new. The old hotel gets torn down; the guests are told to move across the street; none of their furniture fits. This isn't evolution, it's demolition, and it usually arrives as a surprise to everyone who assumed staying current meant staying safe.

---

## Defrosting the Iceberg

Between riding an iceberg forever and abandoning it in a panic, there's a middle path: shrink the iceberg deliberately, ahead of time, so that whenever the day comes the jump is short.

The move is to keep pulling your own work off the framework's shelf and onto your own. Your actual logic — the business rules, the calculations, the things that make your application *yours* rather than just another instance of the framework — can usually be written as plain functions that don't know or care which framework is calling them. The framework then becomes a thin shell around that logic: it handles the routing and the rendering, and at the edges it calls into your framework-agnostic core.

Routing is the clearest example. When routing lives in the framework's own registry, moving frameworks means rewriting all of it. When routing is just files in a folder — this file is this page, the way `about.html` was always the about page — the routing travels with the files, because the position of the file *is* the route. You've handed that job back to the plainest possible layer, and the framework merely reads it.

Do this consistently and the framework's footprint in your codebase shrinks with every new feature, because every new feature's real substance lives in your core, not in the framework's idiom. The iceberg melts from the inside, on your schedule instead of someone else's.

---

## Deprecate the Iceberg, Keep Your Own Code Moving

You don't have to wait for the community to declare a framework dead before you start treating it as legacy. You can deprecate it internally, today, while it's still perfectly alive — by simply not fusing new code to it.

Stop extending the framework's base classes for new work. Stop reaching for framework-specific patterns when a plain function would do. The existing framework code keeps running; nothing breaks. But the new code grows on your own shelf, framework-agnostic, and the share of your codebase that depends on the framework gets smaller over time rather than larger. When the jump-ship day arrives, your core is already extracted, the ice is already thin, and the jump is short.

The two rules that govern all of this are the same two that govern any technology adoption: **never be the first to believe, and never be the last to leave.** Don't board the iceberg that launched last month — its seams are untested, and you'd be discovering its failure modes on your own time. And don't be the one still aboard when it's visibly melting, because the last ones off pay the highest price: the ecosystem has moved, the answers online are years stale, and what would have been a short jump has become a mining operation through compounded layers of frozen dependency.

The question to ask before boarding was never "is this a good framework." It was "can I leave this one when I need to" — and the honest answer is only ever yes if you arrange for it to be yes, from the first day, by keeping your own code yours.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Don't Go Down With Your Iceberg](https://rinie.github.io/2026/06/13/dont-go-down-with-your-iceberg/) on knowing when to leave, [A Billy With Opinions](https://rinie.github.io/2026/07/05/billy-with-opinions/) on the framework that has views about your books, and [There Are No Living Room Bricks](https://rinie.github.io/2026/07/18/living-room-bricks/) on composition and keeping your logic portable.*
