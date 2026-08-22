---
layout: post
title: "Drown, Get Stranded in the Desert, or Move Along the Waterline: Stay Current Is a Verb"
date: 2026-09-01
tags: [gutenberg-semantic, waterline, resolver, evolution, hdfs, perl, obsolescence]
level: general
description: "There are two ways to fail at a waterline, and they're opposites. Cross it perpendicular, into the layer you don't belong in, and you drown. Stand still while it slowly recedes, and you're stranded in the desert. Neither HDFS's team nor Perl 6's team was reasoning from bad information — they just stopped going back to look. Stay current is a verb. It always meant both things at once."
---

[Moving along the waterline](https://rinie.github.io/2026/08/31/moving-along-the-waterline/) — a phone upgrade making an old car's dashboard smarter, the same maps working in a rental car you've never touched — is safe movement, by construction. It stays parallel to the boundary, inside whatever the interface was actually built to support. There are two other ways to move relative to a waterline, and they're the opposite of safe, and they're opposites of each other.

---

## Drowning: Crossing Perpendicular

Drowning is moving *through* the waterline instead of along it — reaching directly into the layer on the other side, bypassing the interface that was supposed to mediate the crossing. Hacking a direct read from a car's raw CAN bus instead of going through CarPlay's actual protocol. Parsing OOXML's internal XML structure by hand, assuming today's exact file layout will hold, instead of going through the format's own API. [Every diamond this series has found](https://rinie.github.io/2026/08/26/the-diamond-moves-it-doesnt-disappear/) — the ORM secretly issuing joins through what looks like a field access, the network database's hardwired pointer chains — is this same failure: coupling directly across a boundary that was supposed to keep the two sides independent, so a change on either side now drags the other one down with it.

You don't drown by moving too slowly. You drown by moving in the wrong direction — down, into water that was never meant to be walked through.

---

## The Desert: Standing Still While the Water Recedes

The opposite failure needs no perpendicular movement at all. Stand exactly where you were, do nothing wrong in any single moment, and the waterline itself can simply move away from you — slowly, usually, which is precisely what makes it easy to miss. One day you're at the water's edge. A decade later you're stranded in the desert, high and dry, disconnected from whatever the boundary has since become, and nothing you did caused it except staying still.

This series has already collected the evidence without naming the pattern. [HDFS colocated storage and compute correctly, for the constraints that existed in 2006](https://rinie.github.io/2026/08/23/s3-out-evolved-hdfs/), and did nothing wrong for a decade — until network bandwidth compounded the waterline somewhere else entirely, and HDFS is now functionally obsolete outside regulated holdouts, not because it broke, but because the water moved on without it. [Perl 6 spent fifteen years perfecting a rewrite](https://rinie.github.io/2026/08/16/perl6-python3-walled-garden/) while Python's ecosystem kept evolving in public, and by the time Perl 6 shipped as Raku, the desert around it was real. [3G-only telematics hardware](https://rinie.github.io/2026/08/10/obsolescence-was-not-an-issue/) did exactly what it was built to do, correctly, for years — until the carriers moved the waterline in 2022 and every car with no bridge underneath it was simply stranded. [Yahoo's human-edited directory](https://rinie.github.io/2026/08/22/neither-tree-nor-deluge/) was the right answer when the web was small enough for editors to keep up — and then it wasn't, not because the directory got worse, but because the water it needed to stay near had moved.

None of these systems drowned. Nothing crossed a boundary it shouldn't have. They just stopped moving, and the waterline — which almost always drifts slowly, a little every year, rarely all at once — eventually left them somewhere the water used to be.

---

## Staying Near the Water

The prescription isn't complicated, even if it's easy to neglect. Move along, not through. And the actual discipline that keeps you near the water has a name already sitting elsewhere in this series: [going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) — watching where the real work is happening, right now, instead of reasoning from a report or a memory of how things used to be. Applied to a waterline, going to the Gemba means physically checking where the boundary currently sits rather than building on your last snapshot of it. The desert isn't a failure of information. It's a failure to refresh it.

None of the stranded systems above were built on bad information at the time. HDFS's team, Perl 6's team, the manufacturers still shipping 3G-only telematics — every one of them was reasoning correctly from an accurate observation, once. What they stopped doing was going back to look again. [The resolver hardening or atrophying argument already made this point about interfaces](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) — it applies just as much to whoever's standing on either side of one, deciding whether to walk back down to the water and check, or trust that it's still where they left it.

Drowning is the fast failure, the one everyone already watches for. The desert is the slow one, invisible in any single year, and it's claimed more systems in this series than drowning ever has — precisely because nobody went back to the Gemba to notice the water had moved.

Stay current is a verb. It always meant both things at once — know facts that are still true, and stay in the water that's actually moving, not the memory of where it used to be. Going to the Gemba, in the end, is just staying current.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Moving Along the Waterline](https://rinie.github.io/2026/08/31/moving-along-the-waterline/) on the safe lateral motion this post contrasts against, [Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on the actual practice of refreshing the observation, and [Borg's Arrogance](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) on interfaces that harden or atrophy the same way the people standing on them do.*
