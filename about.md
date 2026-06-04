---
layout: page
title: About This Series
permalink: /about/
description: "A short map of a long conversation — two layers, the waterline between them, and why it matters for everything from TCP/IP to parking lots."
---

<style>
.post-content { max-width: 72ch; margin-left: auto; margin-right: auto; }
.post-title { text-align: center; }
</style>

This series began as a small question about programming and turned into one idea applied to almost everything — text encoding, browsers, phones, the rise and fall of tech giants, and how to work without fooling yourself. What follows is the through-line, with the heavy technical scaffolding left out.

---

## Two layers, and the line between them

Every system that carries meaning has two layers.

There's the **substrate** — the bytes, blocks, pages, wires, and hardware that *carry* information without *being* it. Think of it as commodity paper: interchangeable, cheap, and fast-changing. (The conversation called this the "Gutenberg" layer, after the printing press.)

And there's the **meaning** — what the information is actually about. This is expensive, particular, and slow to change. (The "Semantic" layer.)

The boundary between them is the **waterline**. Almost everything interesting happens there.

The key fact about the two layers is that *they move at different speeds*. Hardware doubles in capability every couple of years; the meaning of your work — the problem you're solving, the thing your users actually need — changes far more slowly. Substrate is fast. Meaning is slow.

---

## The seam, and the mailing label

Where the two layers meet is a **seam** — an interface. The cleanest image for a good seam is a mailing label: it tells the carrier everything needed to deliver the package and nothing about what's inside. It's the difference between the envelope and the letter — the envelope carries the addressing the postal system needs, the letter carries the meaning meant only for the reader, and keeping the two *separate* is exactly what lets the mail move without anyone having to read your mail. Systems that keep the envelope and the letter cleanly apart stay healthy; systems that glue them together — so you can't handle the addressing without exposing the contents — cause trouble down the line.

A good postman judges the package by its label, not by opening the box. He shouldn't correct your spelling, and he shouldn't even comment on your purchase — both mean he looked inside, and the whole value of a delivery layer is that it stays dumb and indifferent. (This is net neutrality in one picture: deliver the bytes, don't inspect or judge the meaning.)

A seam is only worth having when the two sides move at different speeds. If both sides move at the same pace, the seam is pointless ceremony. The bigger the speed difference across a seam, the more valuable it is — and the waterline is the seam with the biggest difference of all. That's why it's the one to watch.

---

## Cheap moves, not a silver bullet

*(Covered in depth in [Cheap Moves Instead of Silver Bullet](https://rinie.github.io/2026/06/27/cheap-moves-instead-of-silver-bullet/))*

A "silver bullet" is the promise that *one* thing — one tool, one rewrite, one paradigm — is all you need to make everything ten times better. There is no such single fix (an old and correct observation in software), and the mistake is in the promise itself: it aims the demand for a miracle at the wrong layer. You can't get a miracle at the *slow* layer of meaning — but at the *fast* layer of substrate, you don't need a miracle. You need a **cheap move**: a small, repeatable, reversible improvement.

The trick is to keep your work in uniform, independent, swappable pieces — "pages" you can cut between for free, rather than "chapters" where everything is fused and cutting anywhere costs you. Pages are cheap to move, cheap to split across many workers, and cheap to re-home when the ground shifts. Fused, over-optimized things are not: they win *today* and drown when the substrate moves.

This costs something — call it a 10% tax for keeping things loose instead of perfectly optimized for right now. That tax is insurance. "Perfect for now" is really "perfect for yesterday" by the time it ships, because the substrate is always moving. The 10% buys you the freedom to ride the next wave instead of being stranded by it.

A good analogy: don't pave a road for one specific trip; pave it for the traffic. A highway is deliberately *not* optimized for any single journey, and that generality is exactly what lets it carry vehicles that don't exist yet.

---

## Keep your own pace, and keep your exits

A train runs on someone else's timetable — conform or miss it. A car has no schedule but yours. The point isn't "cars good, trains bad"; it's that you should be the one *choosing* per trip, not locked into one operator's schedule for everything. Sometimes the train is the right call. Just don't let it decide your route and call that service.

The same goes for any tool, platform, or service: the question to keep asking is *can I still get out?* A good test for anything cloud-connected: **does it still work with the server switched off?** If yes, you're holding the wheel. If it bricks, you're renting, and someone else holds your pace.

Beware especially the "burning platform" — the rescuer who declares everything on fire and insists the only way out is the single jump he's selling. Sometimes the fire is real, but a true ally fights it *and* keeps your exits open. Anyone who clears the room of every option but theirs isn't saving you.

---

## Why things rise and fall: the same playbook, over and over

A recurring pattern explains a lot of tech history. A scrappy challenger:

1. **rides the latest cheap capability** to wherever the real bottleneck just moved;
2. **keeps the substrate open**, so users can compare it against the old way;
3. **breaks the gatekeeper's fused bundle** by pricing or packaging at the seam users actually want;
4. **wins on the experience** — making the thing genuinely easier to use.

The iPod did this to record labels: it still played your own music files, sold single songs for a dollar instead of forcing whole albums, and made a thousand songs easy to navigate. The iPhone did the same to phone carriers: WiFi let you reach the internet without the carrier's permission (and let you *see* whether the carrier or the phone was the problem), flat-rate data broke the per-minute meter, and the touchscreen made the mobile internet usable. Android then did it to Apple, the way cheap PC clones once did it to IBM: by being the open platform everyone could build on.

The losers all failed the same way. They fused their advantage into something that couldn't change — a closed bus, custom hardware, an exotic design, a walled platform — and when the ground moved, they couldn't move with it. Intel is the clearest case: dominant, unchallenged, and so comfortable it optimized the chip it already had instead of making the leap, until faster, hungrier competitors (kept alive by an open manufacturing market) walked past it. The company that literally wrote the book on staying paranoid fell asleep the moment nothing was chasing it.

That's the underlying law: **no one stays sharp voluntarily.** Vigilance is just what competition feels like from the inside. Remove the competition and the vigilance dies, no matter what the founder believed.

---

## The disease to watch now: the hidden seam

The healthiest version of a recommendation is an honest signal — "people who read this also read that," derived from real behavior, where the seller's interest and yours line up. The disease is when the line between *genuine signal* and *paid placement* gets deliberately blurred, so you can't tell whether something is recommended because it's good or because someone paid for the slot.

This is the difference between a visible price and a hidden one. Paying a premium for a well-made product is a fair trade — you can see the bill and decide. The problem is the hidden cost: the deal where you're being charged (with your attention, your data, your defaults) and not told. The con was never charging for lunch; it's hiding the bill.

The fix is never "the giant reforms." It's always someone re-exposing the hidden seam — showing you which part is the honest signal and which part is the ad — and letting you compare again.

---

## How to work: listen and fix

All of this points to a way of working. Prefer **small, fast iterations over big leaps of faith.** A small step is cheap to undo; a big bet fails all at once with no fallback.

Fix potholes instead of chasing moonshots. A moonshot is really an act of *telling* — here is my grand plan, world please comply. Pothole-fixing is an act of *listening* — reality shows you where it hurts, you fix that, you listen again. The listener ends up somewhere better than the planner aimed for, because ten thousand small corrections beat one big guess. The point is to keep the feedback loop closed and never fall so in love with your own plan that you stop hearing the road.

In short: don't teach the world your ego. Let the world correct it.

---

## A note on honesty

The same logic that explains evolution explains why honesty pays. Deception is a *closed seam*: it works once, and then the trust never comes back — you get routed around forever, the same way a fused product or a complacent monopoly does. Trust is the thing that *compounds*: it's what people return to without having to open the box. In the long run, honesty isn't a handicap; it's the only asset that survives the tide.

And honesty never required you to stay put. You can walk away from a bad deal, a dying platform, or a route that's gone wrong without deceiving anyone — because leaving and lying were never the same act. Stay true to yourself, keep your exits open, and you get to leave clean.

---

---

*This page is an overview of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) — 44 posts and counting, published daily at [rinie.github.io](https://rinie.github.io). The series began with a question about ESLint configurations and ended up somewhere much more interesting.*

---

## The shortest version

Keep your work in cheap, swappable pages. Watch the waterline, where the fast and slow layers meet. Keep the seams honest and visible — deliver the package, don't open the box. Keep your exits open and your own pace in your hands. Prefer small fixes guided by feedback over grand plans driven by ego. And remember that nothing stays sharp without something chasing it — so keep the competition alive, keep the choice real, and keep the river flowing.
