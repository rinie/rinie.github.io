---
layout: post
title: "Watch Your Step: Rocks, Shells, and Soft Mud Near the Waterline"
date: 2026-09-03
tags: [gutenberg-semantic, waterline, breaking-changes, edge-cases, backward-compatibility]
level: general
description: "Strolling isn't careless. There's a category of hazard that's neither drowning nor the desert — small, local, survivable, and specifically the kind of thing attentive strolling catches and marching doesn't. Rocks you can see if you're looking. Shells you can't see until you're already standing on one. Soft mud that looks like solid ground right up until it isn't."
---

[Strolling the waterline](https://rinie.github.io/2026/09/02/strolling-the-waterline/) isn't the same as strolling carelessly. There's a category of hazard that doesn't fit [drowning or the desert](https://rinie.github.io/2026/09/01/drown-desert-or-move-along/) — nothing catastrophic, nothing systemic, just the ordinary small things underfoot exactly where you meant to be walking. Three kinds, worth telling apart.

---

## Rocks

Visible, if you're looking. A documented breaking change sitting right there in the changelog, a known edge case flagged in the migration notes — the information was never hidden, it just requires actually looking down instead of at the horizon. Rocks don't punish curiosity. They punish the assumption that the ground stays uniform because it usually does.

---

## Shells

Small, sharp, and invisible until you're already standing on one. The null check nobody added because the case seemed impossible until it wasn't. The boundary condition that only shows up on the millionth request, not the first thousand tests. Not systemic — a genuine, local cut, right at the edge, that no amount of looking ahead would have caught, because it was never visible from a distance in the first place.

---

## Soft Mud

The dangerous one, because it looks exactly like solid ground until your foot is already sinking. An interface that appears backward-compatible and mostly is. A dependency that looks pinned and isn't quite. Soft mud doesn't announce itself the way a rock or a shell does — there's no moment of "watch out," just footing that gives slightly more than expected, and by the time that's obvious you're already committed to the step.

---

## The Point of Watching Your Feet

None of these are drowning. None of them are the desert. They're the price of admission for being near the water at all, and the only real defence is exactly what strolling already implies — attention at the pace of a walk, not a march. A march looks at the destination and trusts the ground. A stroll looks at the ground, because that's most of the point of strolling in the first place.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Strolling the Waterline](https://rinie.github.io/2026/09/02/strolling-the-waterline/) on the disposition this post keeps honest, and [Drown, Get Stranded in the Desert, or Move Along the Waterline](https://rinie.github.io/2026/09/01/drown-desert-or-move-along/) on the two larger failures these smaller hazards are not.*
