---
layout: post
title: "Worse Ages Better Than Perfect"
date: 2026-06-28
tags: [worse-is-better, pace-layering, gutenberg, portability, stewart-brand, own-vs-rent]
level: general
description: "The perfectly designed thing tends to be perfect for yesterday. It spent its whole budget being correct about the requirements as they were when it was designed, and has nothing left over for moving when the ground shifts. Simplicity at the seam is portability through time."
---

## Gutenberg was not a brilliant writer

Johannes Gutenberg was not a brilliant writer. His press produced output that a skilled scribe would have considered rough, mechanical, and aesthetically inferior. The illuminated manuscript was more beautiful, more carefully considered, more precise — every letter formed by hand, every margin decorated, every page a small artwork. The printed page was cheaper, faster, and slightly wrong in ways that craftsmen noticed and readers eventually stopped caring about.

The monks who copied manuscripts were the original gatekeepers. Not malicious — genuinely skilled, genuinely devoted. But the selection was theirs: the text that got copied was the text the monastery valued, the interpretation that survived was the interpretation the copyist endorsed, the marginal note became part of the manuscript. The Def was fused to the content. There was no clean seam between the carrier (the monk, the scriptorium) and the meaning. If you wanted a book, you wanted what the monastery had decided to copy.

Gutenberg bypassed the monks by making the carrier mechanical and indifferent. The press did not have opinions about the text. The type did not endorse the argument. The page did not add marginalia. The Gutenberg layer became genuinely Gutenberg — physical, positional, content-free. The meaning could travel without the carrier's endorsement.

The result was not immediately better writing. It was 10x more readers. And some of those readers became writers. The feedback loop opened. Use-Pull arrived. The Semantic layer compounded because the Gutenberg layer stopped gatekeeping it.

The illuminated manuscript was perfect. It was also fused to its production method. When the substrate shifted, it could not follow — the beauty was inseparable from the making. The printed page was worse. It enabled everything that came after.

**Newspapers as Def-Push that learned:**

The newspaper editor still chose your headline. Still framed the story. Still decided what was news. But competition arrived — multiple papers, circulation numbers, the reader who cancelled, the letter to the editor. These were Use signals. The editor who ignored them lost readers. The editor who heard them became a better editor. Def-Push with a feedback loop, calibrating toward the Use over time. Mostly harmless.

The full arc: monks (Def-Push absolute, no exit) → Gutenberg (carrier bypassed, meaning travels) → 10x readers (Use-Pull emerges, readers become writers) → newspapers (Def-Push with competition, mostly harmless) → internet (both simultaneously, resolver is the new gatekeeper) → enshittification (resolver captured, Def-Push reasserts) → open protocols (bypass again, same move, different century).

The move that works is always the same: make the carrier indifferent, let the meaning travel, keep the seam clean. The monks were not the problem. The fusion was.

**The monks and their patrons are still here.** The engineer who designs without a feedback loop, the designer who ships without watching the 90% struggle, the platform that owns the resolver and stops listening — all monks, all patrons. The printing press did not make them go away. It made their failure mode visible by contrast. The system that has no variation and ignores the 90% signal always gets routed around eventually — not punished, just bypassed. The carrier that stays indifferent wins.

Let the 90% win.

---

## Two honest temperaments

There's an old split in how people build things, and it's worth stating fairly, because both sides are sincere and both are sometimes right.

One temperament wants to **own the whole stack and design every seam correctly** — do it properly, top to bottom, make each interface right. Call it the "right thing." The other wants, above all, to **run the same program on a different computer** — to ask as little as possible of the substrate and conform to an interface others already share, even if the result is a little rough at the edges. Call it "worse is better."

The "right thing" is not wrong. For a genuine one-off — a thing you'll own and live inside for decades, where there's no commodity to conform to anyway — designing every seam yourself is exactly correct. You *should* build your own house that way. The trouble is only that owning every seam quietly fuses you to your own stack: nobody else's parts fit your bespoke interfaces, and a seam only you speak is a seam you can't swap across. The cost is paid later, and it's paid as time.

Because here's the thing that makes "worse is better" win the long game: **the perfectly designed thing tends to be *perfect for yesterday*.** Not through any failure of skill — through timing. It spent its whole budget being correct and complete about the requirements *as they were when it was designed*, and so it has nothing left over for moving when the ground shifts. The rougher, simpler, more swappable thing left slack — and that unspent budget is exactly what it later uses to adapt. Simplicity at the seam is portability through time. "Better" sucks in five years because better was fused to a moment, and the moment passes.

A small, beautiful example from computing history: early filesystems faced a dilemma — big storage blocks are fast but waste space on small files; small blocks save space but are slow. The elegant "right" answer would handle every size uniformly and gracefully. The Berkeley filesystem did something cruder and smarter instead: use big blocks for the whole file *except the last one*, which can be a fragment. Pay the irregularity only at the boundary; keep the body uniform and fast. It was slightly ugly, it shipped, and it ran the world for years. The unspent elegance was the slack that let it actually exist.

---

## Credit where it's due: Brand and the learning building

Stewart Brand is sometimes invoked as a caution, and that's unfair to him. His book *How Buildings Learn* is one of the great arguments *for* this whole stance, not against it.

Brand's observation was that architects design for the photograph — the building as it looks on opening day — and then the building has to survive being *lived in*, and the two are at war. The award-winning, high-design buildings often resist adaptation: the vision is so complete that the inhabitants can't change anything without fighting it. Meanwhile the plain, cheap, ordinary buildings **learn** — they get repartitioned, added to, repurposed across decades — precisely because nobody fused them to a single vision, so the people using them could pull them into new shapes.

That is the core idea in masonry: a building is not finished when it's built; it's finished, continuously and never, by the people who use it. The architect who thinks the design is *done* on opening day is the same ego as the engineer who won't let users correct the system. Brand's villain is that ego. His hero is the loose, adaptable, "worse" building that leaves room to be corrected. He was on the side of *listen and adapt* all along.

(The thing that sometimes gets sold as a caution isn't Brand — it's what happens when his descriptive insight about how things *age* gets packaged by consultants into a prescription to go *architect your own bespoke layered system*. That repackaging quietly smuggles back the "own every seam" trap. The insight is Brand's and it's sound; the sales pitch built on top of it is the part to distrust.)

The one thing a building can't do, however well it learns, is *leave*. It's nailed to its site. Which sets up the distinction that tells you when to own and when to rent.

---

## Own the house, rent the laptop

The question that decides everything is simple: **is this a one-off you live inside, or a commodity you live on?**

A house is singular, long-lived, site-bound, and yours. Owning its seams pays. But you don't build your own laptop, car, or phone — those are mass-produced, identical, used by millions, and evolving on a curve no individual could match. Trying to own those seams would be insane; you'd be hand-making what a global industry makes a billion of. For those, you conform to the commodity and ride the curve.

The expensive mistake is treating a laptop like a house — building your own bespoke platform when you should have bought the standard one. Almost nothing is a house. Almost everything is a laptop.

And even *hardware* — once the ultimate build-it-all-yourself domain — broke into commodity seams, and the breaking is what let it keep racing ahead. Chip *designs* became licensable or open (Arm, and now RISC-V, designed everywhere by anyone), *fabrication* became a shared service (one foundry serving hundreds of customers), and *assembly* became a commodity supply chain. The vertically integrated hardware "house" got unbundled into rentable rooms, and that unbundling is exactly why hardware kept improving instead of fossilizing.

The same move runs all the way up the modern stack. Separate compute from storage and you can swap the processor — even run it locally — without moving the data. Put your data behind a standard storage interface and the *provider* becomes swappable too: you reap the benefit of one cloud today and move to a cheaper one tomorrow at low cost. The lesson is always the same: **don't fuse to the implementation; conform to the interface, and let the things below it compete for you.** A thing you can't swap is a thing that can't evolve — and that's true whether it's a cloud vendor or a tightly-coupled piece of your own code.

---

## Mobility beats adaptation; the plate beats the car

A learning building adapts where it stands. A car and a shipping container do something better: they *move*. The container's genius was never the box — it was that the box is a standard unit, so the cargo inside rides untouched while the ship, the crane, the truck, the port underneath all get swapped at every leg. Standardize the seam and the thing inside can take *any* road, re-routed cheaply the moment one is blocked.

Stretch that across time and you get the whole five-year stance in one image: the next car, the next phone, the next laptop — while you keep the **license plate**. The plate is the identity that outlives the vehicle; it moves across every upgrade unchanged. Commit to the durable handle at the seam — your name, your data, your account, the standard interface — and treat everything below it as this year's disposable steel. Each "next" then becomes free upside you collect by *not* having married the "now."

So: hold the plate, spend the cars. Keep your exits open. A good test for anything you depend on — does it still work with the server switched off? If yes, you're driving. If it bricks, you're renting your own pace from someone else.

---

## How you actually know: walk and adjust

None of this is a blueprint to be defended. The working model is deliberately small: **the world is a paged stream of bytes, and you go to where the work actually happens to look at it.** ("Go to the gemba," in the Lean phrase — stand on the floor, watch the real flow, don't theorize from the office.)

You don't decree the structure up front and you don't inherit it as fixed. You *walk along the stream and adjust the page size as the work pushes back* — too coarse and you waste the edges, too fine and you drown in overhead, and the right size is the one the real work reveals as you move through it. The layers are learned from feedback, not designed in advance. You keep a *sense* of where the seam is — held loosely enough that the next step can move it.

Two principles hold it together. **Practice corrects the theory** — the arrow runs upward, from contact with the real to the model, never the other way. And **the user holds it correct** — correctness isn't a property your design has on paper; it's a verdict the user renders in use. The user is the one part of the system you can't fake a "yes" from, which is exactly why they're the right judge.

Which sets the ethic: **the user doesn't suck — you can reduce waste.** Every instinct to blame the user ("they're doing it wrong, they should be retrained") is misplaced contempt; the fixable problem is almost always waste in the flow around them, and the waste is yours to remove. Respecting the user isn't sentiment. It's just correctly locating where the problem you can actually fix lives.

---

## The shortest version

The worse model is better — because a model loose enough to call itself worse is a model that practice can still correct. Own the genuine one-off; rent the commodity. Conform to the shared seam so you can move, and keep the plate while the vehicles change. Walk the stream, adjust the page as the work teaches you, and let the user hold the verdict. Don't fuse what you'll want to swap, and don't optimize so perfectly for now that you've built something perfect for yesterday.

And when the substrate does something dumb and sudden — as it will — the oldest advice on the best-known cover still holds: don't panic. Keep your towel, keep your exits, and keep walking.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Cheap Moves Instead of Silver Bullet](https://rinie.github.io/2026/06/27/cheap-moves-instead-of-silver-bullet/) on why the leverage is at the fast layer, [Don't Go Down With Your Iceberg](https://rinie.github.io/2026/06/13/dont-go-down-with-your-iceberg/) on the cost of fusing to the substrate, and [Gutenberg: Your Next Phone Will Be a Different Make on a Different Carrier. Your Number Is Still Yours.](https://rinie.github.io/2026/06/15/gutenberg-your-next-phone/) on holding the plate while the vehicles change.*
