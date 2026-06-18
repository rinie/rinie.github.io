---
layout: post
title: "Brooks Named the Waterline in 1986"
date: 2026-07-24
tags: [gutenberg-semantic, brooks, pdca, lean, ai, essential-complexity, waterline]
level: general
description: "Fred Brooks split software complexity into essential and accidental forty years ago. AI eats the accidental kind — eight times more code, only a third more releases. The bricks were never the bottleneck. And if your check concludes the plan was wrong, eight times the bricks just builds the wrong thing faster."
---

In 1986, Fred Brooks wrote an essay called "No Silver Bullet," and the distinction at its centre is the same one this series has spent seventy posts re-deriving from printing presses, phone numbers, and flat-pack furniture. He just got there forty years earlier and used different words.

Brooks split the difficulty of building software into two kinds. **Accidental complexity** is the friction of the tools — the clunky language, the slow compiler, the boilerplate, the limitations of whatever technology you happen to be using this decade. It gets better over time, because better tools chip away at it. **Essential complexity** is the difficulty of the problem itself — figuring out what the software should actually do, for whom, under what conditions, and what "correct" even means. Better tools do not touch it, because it was never about the tools.

His conclusion followed directly: since any new technology can only reduce the accidental kind, no single tool will ever produce an order-of-magnitude leap in how fast software gets built. There is no silver bullet, because the wolf was never the tooling.

That is the Gutenberg/Semantic split, stated in 1986. Accidental complexity is the Gutenberg layer — the carrier, the machinery, the part that Moore's Law and better tooling and now AI keep eroding. Essential complexity is the Semantic layer — the meaning, the deciding, the part that stays hard no matter how good the carrier gets. Brooks named the waterline before the series did.

---

## Eight Times the Bricks, One-Point-Three Times the Rooms

The reason this is worth revisiting now is that we are running Brooks's experiment at scale, in public, with AI writing code.

The numbers that have come out of it are the cleanest confirmation of his argument anyone could have asked for. In one widely-cited account, AI coding agents produced roughly eight times as many lines of code — and only about thirty percent more actual releases. Eight times the bricks. One-point-three times the rooms.

If that gap surprises you, it shouldn't, because it is the whole point of the series restated in production data. Writing the code was never the bottleneck. The bricks were never the bottleneck. More Gutenberg throughput does not produce proportionally more Semantic output, because the Semantic output — the deciding, the verifying, the figuring out whether this was even the right thing to build — was never waiting on how fast the bricks got laid. It was waiting on the part AI does not compress.

Think of the work as three stages: **Decide** what to build, **Execute** the building, **Deliver** something verified and accountable. AI almost entirely compresses the middle one. Execute was the accidental complexity — the typing, the boilerplate, the syntax. The other two are essential, and they are exactly where the eight-times-faster bricks pile up against a wall.

---

## The Plan, the Do, and the Check That Has to Watch Both

There is an old quality-improvement loop called PDCA: Plan, Do, Check, Act. You plan what to do, you do it, you check whether it worked, and you act on what the check told you. It sounds obvious. The interesting part is a question most people skip: when the check comes back bad, *what* do you fix?

There are two answers, and they define two completely different ways of working.

The first answer assumes the plan was right, so a bad check can only mean the doing was sloppy — execute the plan again, harder, more carefully. Call it the everything-was-specified-correctly stance. In this world the plan is never the thing that's wrong, so the check can only ever point at the doing.

The second answer treats the plan as a hypothesis, which means a bad check might be telling you the doing was sloppy *or* that the plan itself was wrong. The check has to validate both. And when the check concludes the plan was wrong, you fix the plan — not the doing.

That second answer is the one this series has argued for since the post on waste, and it is the one the AI numbers vindicate. Because here is what eight-times-faster execution does to each stance.

If you believe the plan is always right, cheap execution is pure upside: do more, faster, forever. If you believe the plan is a hypothesis, cheap execution is a loaded question: you are now generating eight times as much code against a plan nobody has checked yet. When the check finally runs and says the plan was wrong, you have eight times as much wrong thing to throw away.

**If the check concludes the plan was wrong, you correct the plan.** Not the doing. Pouring more execution into an unchecked plan is just building the wrong thing faster — the volume changes, the correctness does not. Eight times the bricks, laid perfectly, in the shape of a house nobody wanted.

---

## Why Small Bets Beat Big Plans, Especially Now

This is also the real reason fast iteration beats big upfront planning, and the reason isn't that fast is virtuous for its own sake.

A big plan is a large bet that the plan is correct, placed before any check has run. The bigger the plan, the more doing piles up before anyone checks it, and the more it costs when the check finally says the plan was wrong — because now there's a mountain of work built on a wrong foundation to unwind.

Fast iteration is a series of small bets: small plan, small amount of doing, check quickly, fix whatever was wrong while the cost of being wrong is still small. It prefers frequent cheap checks precisely because "the plan was wrong" is then a survivable Tuesday instead of a catastrophe.

Now layer AI on top. Cheap execution means you *can* generate enormous amounts of work per iteration. The instinct is to do exactly that — finally, the bricks are free, lay millions of them. But that turns each iteration into a bigger bet against an unchecked plan, which is precisely backwards from what the cheap execution should let you do. The right move with newly-cheap execution is more, smaller checks — not more, bigger bets.

---

## The Job That's Left

If AI compresses the Execute layer to nearly nothing, the human job is what's on either side of it: deciding what's worth building, and verifying that what got built was right and is accountable. One way it's been put is that the engineer becomes something like a crane operator — the machine does the heavy lifting, and the human's job is steering it and keeping it under control.

In this series' terms, that is the resolver's job. Not laying the bricks — that's compressed — but checking that the wall being built matches the floor plan, and that the floor plan still matches what the person who has to live there actually needs. It is the lead engineer measuring waste, except the waste is now generated eight times faster, so the checking has to keep pace or the gap just fills with confidently-built wrong things.

The work that survives is the work Brooks called essential in 1986: deciding and verifying, the two ends of the sandwich that the machine in the middle cannot taste. The bricks got cheap. They were never what was expensive.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Cheap Moves Instead of a Silver Bullet](https://rinie.github.io/2026/06/27/cheap-moves-silver-bullet/) on Brooks and where the leverage actually is, [More Than 10% Waste? That Was a Leap of Faith](https://rinie.github.io/2026/07/15/more-than-10-percent-waste/) on the check that has to validate the plan and not just the doing, and [There Are No Living Room Bricks](https://rinie.github.io/2026/07/18/living-room-bricks/) on keeping your own logic on your own shelf.*
