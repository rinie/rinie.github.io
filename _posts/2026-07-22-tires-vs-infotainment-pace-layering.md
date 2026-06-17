---
layout: post
title: "Tires 3.0, Oil 12.0, but (M)Apps Stuck at 1.0"
date: 2026-07-22
tags: [gutenberg-semantic, pace-layering, cars, standards, seams, infotainment]
level: general
description: "By Stewart Brand's logic, infotainment software should be the fastest-changing part of a car and tires the slowest. The opposite happened. Tires, oil, and fuel got an open, regulated physical seam decades ago. Infotainment never got an equivalent seam at all — it shipped fused to one vertically integrated stack, with no obligation on anyone to keep it current once the warranty lapsed."
---

Your car is five years old. The tires have been replaced twice, by whichever shop was closest, with tires from a brand that didn't exist when the car was built. The oil has been changed a dozen times, by three different garages, none of them affiliated with the manufacturer. The fuel comes from whichever station was on the way home. You can change all three on your own schedule, from whichever supplier you like, without asking anyone's permission.

The infotainment system still has the maps it shipped with. The road that opened two years ago doesn't exist on it. There is no upgrade path, no aftermarket replacement that bolts in cleanly, and the manufacturer's answer, if you ask, is usually some version of "buy the next model." Maps are apps — the M is silent but it shouldn't be, because a map that can't be patched is exactly the same failure as an app that can't be patched, and you can't fix either one yourself.

By the logic this series has been building since Stewart Brand's shearing layers first appeared in it, this is backwards.

---

## What Pace Layering Predicts

Brand's layers run slow at the bottom, fast at the top. Site and Structure barely move. Skin and Services move on a renovation cycle. Space Plan and Stuff — furniture, devices, anything you can carry out the door — move constantly, because they're cheap to replace and nobody needs permission to do it.

Apply that to a car and the prediction is straightforward. Tires, oil, and fuel are physically consumed and have to be replaced on a schedule whether anyone plans for it or not — they should behave like a fast layer, churning constantly, open to whoever wants to compete for the replacement business. Infotainment is software. Software is supposed to be the fastest layer there is — no physical wear, no consumable mass, nothing stopping an update except a download. By pace-layering logic, infotainment should out-evolve the tires by a wide margin.

It doesn't. The tires get a thriving, multi-brand, instantly-competitive aftermarket. The infotainment system gets frozen the day the car leaves the factory floor, give or take a recall.

---

## The Difference Is the Seam, Not the Material

Tires, oil, and fuel aren't fast-moving because they're physical. They're fast-moving because, decades ago, someone forced an **open, standardized interface** onto each of them, and that interface has nothing to do with any single manufacturer's goodwill.

A tire's bead diameter, width, and load rating are standardized — a 225/45R17 means the same thing whether the tire comes from a company that's been making tires since 1900 or one that started last year. Any manufacturer that meets the published dimensions and safety ratings can sell into the slot your car already has. The seam is the wheel well and the bolt pattern, and that seam has been stable, public, and enforced by regulation and industry standards bodies for generations. Oil viscosity grades (5W-30, 10W-40) and fuel octane ratings work the same way — published numbers, checkable at the pump or on the bottle, with no proprietary lock-in attached to the number itself. The interface is open. Competition does the rest. The layer behaves fast because the seam was built to let it.

Infotainment has no equivalent. The screen, the operating system underneath it, the map data format, and the head unit hardware are typically one vertically integrated stack, owned end to end by the manufacturer or a single supplier the manufacturer chose, with no published interface that a third party could target the way a tire company targets "225/45R17." There's no regulatory body insisting that map data must be swappable, or that the head unit must accept software from any vendor who meets a spec. The system is "software" in the sense that it's made of bits, but it shipped fused — to the hardware, to a licensing agreement, to whatever mapping provider the manufacturer signed a contract with at the time — and nothing in that arrangement obligates anyone to keep updating it once the car has stopped being profitable for the manufacturer to support.

---

## Physical Commoditization Beat Software Currency

This is the part worth sitting with: the layer everyone would have predicted should age worst — rubber, liquid, things that get used up and replaced by necessity — turned out to age *best*, precisely because someone forced a Chesterton's-Fence-shaped seam onto it before the cars were even built. The layer everyone would have predicted should age best — pure software, the thing that costs nothing physical to update — turned out to age worst, because nobody ever forced an equivalent seam onto it, and a vertically integrated stack with no externally enforced interface has no structural reason to keep moving once the warranty period that motivated its maintenance has expired.

Pace layering describes how fast a layer *can* move, given its physical constraints. It doesn't guarantee that a layer will actually move at that speed. The seam does that. A fast layer with no open seam is just a layer with no excuse — it could move quickly, nothing physically prevents it, and it doesn't, because nobody built the interface that would let anyone other than the original manufacturer try.

---

## The Maps Are Five Years Old Because No One Was Required to Update Them

Tire manufacturers compete to sell you a tire because the seam lets them. Map providers don't compete to sell your infotainment system an update, because there's no seam for them to compete through — only a contract between your specific manufacturer and whichever provider they picked when the dashboard was designed, with no obligation on either party once that contract's term, or the warranty, runs out.

The fix isn't "make infotainment more like software" — it already is software, that was never the missing ingredient. The fix is the same one tires got two generations before anyone thought to ask for it: an interface specified clearly enough, and enforced widely enough, that a manufacturer's reluctance to keep the system current stops mattering, because someone else is allowed to build the part that replaces it.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Knowing Your Craft: Adding an Architect Slows Down, Adding a Bricklayer Speeds Up](https://rinie.github.io/2026/07/14/knowing-your-craft/) on craft-based shearing layers, [Frameworks Move the Seam: Even Trains Don't Demand a Wider Track](https://rinie.github.io/2026/07/06/frameworks-move-the-seam/) on the cost of an unstandardized interface, and [Your Email Address Is Hostage](https://rinie.github.io/2026/05/25/email-address-hostage/) on what it costs to be locked to an infrastructure provider with no external resolver.*
