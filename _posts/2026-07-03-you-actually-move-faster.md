---
layout: post
title: "You Actually Move Faster Without Breaking Things"
date: 2026-07-03
tags: [gutenberg-semantic, rewrite, dual-track, schmiel, deprecation, pace-layering]
level: general
description: "Move fast and break things is only fast on the first lap. Every subsequent lap is slower because you are navigating the debris from the last one. Slow and smooth compounds. The pothole fixes accumulate. The floor stays clean. Over five years, the team that never broke anything built more than the team that moved fast."
---

"Move fast and break things" was Facebook's explicit claim. Ship fast, iterate, fix later. It worked for a social network where the worst case was a confusing UI update that annoyed users for a week.

It does not work for an operating system. Or a compiler. Or a database. Or a payment system. Or any infrastructure someone else depends on.

The faster you move the more Chesterton's Fences you knock down without knowing they were load-bearing. The more you break the more time you spend fixing what you smudged instead of building the next thing. Fast and breaking things is only fast on the first lap. Every subsequent lap is slower because you are navigating the debris from the previous one.

Slow and smooth is actually faster over five years.

---

## Schmiel the Painter

Joel Spolsky wrote about Schmiel the painter in 2000. Schmiel painted himself into a corner and escaped by painting over his own footsteps — leaving a trail of smudged paint across the floor he had just finished.

The classic rewrite. The legacy codebase is the corner. The rewrite is painting over the footsteps. You escape the corner but you smudge everything you already did, and the new code has all the same problems as the old code plus the new bugs you introduced, minus all the accumulated wisdom that was encoded in the old code's weird edge cases.

The strange `if` statement on line 1,247 that looks like it should not be there — it is there because of a customer in 2009 who had a specific configuration that broke everything, and someone fixed it at 2am on a Friday and it has been quietly saving you ever since. Rewrite from scratch and you lose it. You will rediscover it in 2027 when the same customer calls again.

This is a hard lesson for non-technical people who think the next version will be clean. It will not be clean. It will be clean until it encounters the same reality the old version already survived.

Netscape Navigator was the dominant browser. The codebase was old, layered, full of Schmiel's footsteps. The decision: rewrite from scratch. Three years. Shipped something no better than what they had. Internet Explorer had the market by then. The smudges were everywhere and the floor was lost.

---

## The Old Code Is Not a Mess. It Is a Map.

The old code looks like a mess because it was written by people under pressure, in sequence, without knowing what would come next. That is also why it works.

Every strange conditional is a Chesterton's Fence — a boundary that looks arbitrary until you understand why it was built. Remove it and something breaks that you did not know depended on it. The 2009 customer. The timezone edge case. The integer overflow that only occurs on leap years. The behaviour that three integrations rely on without documenting.

The old code is not embarrassing technical debt. It is load-bearing archaeology. The rewrite that clears it loses all of that. Not because the old code was well-written — it probably was not — but because it was correct. Correct in ways nobody wrote down because nobody knew they needed to write them down at the time.

The photograph of the new, clean codebase looks better than the old one. In production, the old one is keeping the customer on the line.

---

## The 386 Move

Intel faced the same problem in 1985. The 8086 segment model was the corner. Real mode, protected mode on the 286 that was a one-way trap door, the near/far/huge pointer mess — decades of accumulated decisions that were individually defensible and collectively a dead end.

Intel did not rewrite. They did not deprecate. They added a new mode alongside the old one — protected flat 32-bit mode on the 386, where segments were still there but could be ignored. DOS programs kept running. The installed base was preserved. Developers moved to flat mode when they were ready, not because they were forced.

The weird segments are still in x86-64 today. Still there. Just not used. The floor has layers of old paint. The new paint is on top. The customer with the specific configuration from 2009 still works because the old layer is still there underneath.

This is the correct move: **add a new mode alongside the old one, make the new mode better, let the installed base migrate at human pace.** The old mode does not need to be deprecated. It just stops being used. Eventually it is unused — not deleted, not smudged, just irrelevant. The floor stays clean.

Non-technical management sees the layers and wants to clean them up. "Why do we still have this old code? It is a mess. We should rewrite it." The technical answer: because it contains things we do not know we need yet. The floor only looks clean in the photograph. In production it is load-bearing.

---

## The Kernel as the Proof

Linus Torvalds runs the Linux kernel on one explicit rule: we do not break user space. Ever.

A patch that makes the kernel more elegant but breaks a user-space program is rejected. Regardless of how correct the kernel change is. Regardless of how ugly the workaround needed to preserve compatibility. The Gutenberg layer improves continuously. The semantic layer above it never notices.

Thirty years. Billions of devices. No Schmiel moves. Not because the kernel is perfect — it is full of the architectural equivalent of line 1,247. But because every line that looks wrong is probably there for a reason, and the rule that protects it is worth more than the elegance of removing it.

The kernel is slow and smooth. It is also the most widely deployed operating system in history. Your phone, your server, your router, your television. All running the kernel that refused to break things even when breaking things would have been cleaner.

---

## Slow and Smooth Compounds

The pothole fix is small. Cheap. Unsexy. Nobody announces a pothole fix. Nobody writes a blog post about it. The sprint board does not have a card called "fixed the thing that has been slightly wrong since 2019."

But pothole fixes compound. Each one removes a small friction. Each one closes a small gap between what the system does and what the users need. After a year of pothole fixes the system is measurably better — not in any one dramatic way, but in the aggregate way that makes users trust it. The 2009 customer does not call on Friday. The timezone edge case does not surface in a production incident. The leap year integer overflow stays hypothetical.

The moonshot, the rewrite, the "fast and break things" sprint — each of these has a cost that is not visible until after the move. The smudges on the floor. The Chesterton's Fences lying in the debris. The users who depended on the old behaviour discovering the dependency the hard way.

**Fast and breaking things is debt.** You borrow speed from the future. The interest is paid in every subsequent lap that is slower because the debris from the last lap is still on the road.

Slow and smooth is investment. Each pothole fix is a small payment now for a road that stays drivable. The team that never broke anything built more over five years than the team that moved fast — because they were building on top of what they built before, not navigating around what they smudged.

---

## You Know the Next Turn. Not the Destination.

Eric Ries's Lean Startup makes the same point at the product level. The minimum viable product is not a shortcut — it is the cheapest possible test of a hypothesis about where you are going. Ship the smallest thing that generates the real Use signal. Let the signal tell you the next turn.

Not the destination. The next turn.

"Move fast and break things" assumes you know the destination. The direction is fixed. The speed is the variable. The Lean Startup says the destination is a hypothesis. The blueprint assumes knowledge you do not have yet. The MVP uses knowledge you actually have — and generates the knowledge you need for the turn after that.

Ries draws the sharpest line here: **vanity metrics versus validated learning.** Vanity metrics are the Def deciding what success looks like — page views, sprint velocity, features shipped. All countable, all controllable by the Def without any Use signal required. You can increase page views by making the site slower so users reload more often. The metric goes up. The experience gets worse.

Validated learning closes the loop. Not "we shipped 47 features this sprint" but "we believed users were abandoning the checkout because of the payment step — we changed it — abandonment dropped 23% — the hypothesis was right." Reality answered. The measurement was honest. The next turn became visible.

Slow and smooth runs on validated learning, not vanity metrics. Each pothole fix is a hypothesis: this is causing friction, removing it will help. Watch whether it helped. If it did, the next turn is clearer. If it did not, the road tells you where to look next. You do not know which Chesterton's Fence is load-bearing until you remove it. You do not know what the system needs to become until the users show you.

The 386 knew the next turn: flat 32-bit mode, compatible with the installed base. It did not know the destination was Linux on Android running analytical queries faster than a 2004 research cluster. Nobody could have known that in 1985. The next turn was enough. The turn after that arrived when the Gutenberg layer crossed the next threshold and the semantic layer above the waterline collected the gains for free.

You do not move fast by knowing the destination. You move fast by keeping the road drivable so you can take the next turn when it appears.

Slow and smooth. The floor stays clean. The next turn comes into view.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Joel Spolsky's original: [Things You Should Never Do, Part I](https://www.joelonsoftware.com/2000/04/06/things-you-should-never-do-part-i/). Related: [Deprecation Considered Harmful](https://rinie.github.io/2026/06/07/deprecation-considered-harmful/) on old stars fading without being outlawed, [Cheap Moves Instead of Silver Bullet](https://rinie.github.io/2026/06/27/cheap-moves-instead-of-silver-bullet/) on pothole fixes compounding, and [The Watershed](https://rinie.github.io/2026/07/02/the-watershed/) on the 386 move as the correct response to a dead end.*
