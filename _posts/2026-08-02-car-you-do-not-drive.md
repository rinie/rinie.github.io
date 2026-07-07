---
layout: post
title: "A Car You Do Not Drive"
date: 2026-08-02
tags: [gutenberg-semantic, resolver, rail, mit-pdca, new-jersey-pdca, waterline]
level: general
description: "A hi-rail truck has road wheels and rail wheels. On the road, the driver resolves the route continuously — every intersection is a live decision. On the rails, the wheels are engaged and steering disappears entirely. The path was fixed the day the track was laid. Same vehicle, same driver, two completely different relationships to the seam."
---

![A CN hi-rail maintenance truck straddling a railway crossing, with a yellow CAUTION RAILWAY CROSSING gate in the foreground](/assets/img/hi-rail-truck-crossing.png)

This is a hi-rail truck — a maintenance vehicle with ordinary road tires and a second set of retractable steel wheels that drop down and engage the rail. It can drive on the road like any other truck. It can also, at the flip of a switch, become a train.

The photo catches it mid-crossing, the exact seam between the two modes.

---

## Road Mode: The Driver Resolves Every Intersection

On the road, the truck's relationship to its path is exactly the [Use-Pull model](https://rinie.github.io/2026/06/17/going-to-the-gemba/) the series keeps returning to. Every intersection is a live decision. The driver sees traffic, sees the closed lane, sees the tractor pulled halfway onto the shoulder, and re-resolves the route in real time. Nothing about the road forces a single path. The Semantic layer — judgment, context, "what's actually happening right now" — is doing all the work. The tarmac is indifferent to where you point the wheel.

This is the driver as resolver. The path is never fixed in advance. It's decided continuously, by someone watching the actual situation, the way a GPS re-routes around the accident it just heard about.

---

## Rail Mode: The Path Was Fixed the Day the Track Was Laid

Drop the rail wheels and steering disappears. Not "becomes harder" — disappears. The driver still controls throttle and brake, but the question "which way do I turn?" no longer exists as a question. The rail is the answer, permanently, for every truck that will ever engage it.

This is [MIT-PDCA](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) taken to its physical limit. Full specification, zero flexibility, and — crucially — it works. It works because the domain was genuinely knowable in advance: trains go where the track goes, that fact was settled by a survey crew years ago, and no amount of daily judgment improves on it. Borg's arrogance ("we will just recompile the iceberg every night") only holds up when the iceberg really is fully specifiable. A railway is one of the rare cases where it actually is. Nobody needs the resolver to be smart at 3pm on a Tuesday, because the resolver already did its one job, permanently, at survey time.

---

## The Seam Is the Crossing

The yellow gate in the photo — CAUTION RAILWAY CROSSING — is the seam itself, and it's marked exactly the way [a load-bearing boundary should be](https://rinie.github.io/2026/07/27/dont-hide-the-fence/): visible from a distance, unmissable, not shouting louder than the situation warrants. It doesn't need to, because the two modes on either side of it are so completely different that hiding the boundary would be dangerous rather than merely inconvenient. A driver who doesn't notice the crossing is a driver about to meet a system with zero steering.

The truck in the photo is doing something most vehicles never have to: it carries both models at once, switches between them deliberately, and the switch itself is the most dangerous moment in its whole operation — not because either mode is unsafe, but because the crossing is where a Use-Pull driver's instincts (there's always another way around) meet a Gutenberg-Push track (there is no other way, the path was decided years ago).

Most systems only ever get one of these models. A car that's always resolved live. A train that's never resolved at all. The hi-rail truck is the rare case that has to be honest, out loud, about which one it's currently running — because getting that wrong at 50 kilometres an hour is not a metaphor.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Borg's Arrogance](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) on systems specified once and never revisited, [Don't Erase the White Line. Don't Pour Concrete Either.](https://rinie.github.io/2026/07/27/dont-hide-the-fence/) on marking a seam in proportion to what it's carrying, and [Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on the driver who resolves the route by watching the actual road.*
