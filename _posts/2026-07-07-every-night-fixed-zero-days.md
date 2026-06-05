---
layout: post
title: "Every Night, Fixed Zero Days"
date: 2026-07-07
tags: [gutenberg-semantic, moores-law, security, potholes, zero-days, rust, fusion]
level: general
description: "Every night the browser patches a zero-day. Every week VS Code improves. Every morning life goes on mostly unchanged. Potholes are small fixes. Zero-days are sinkholes — nobody designed them but you still have to fix them. The Gutenberg layer improving silently while the Semantic layer never notices is Moore's Law working as designed. Fusion and Mars are still thirty years away because they are not pothole problems."
---

Every night the browser fixes a zero-day. Every week VS Code ships an update. Every morning you open your laptop and life goes on mostly unchanged.

This is not an accident. It is the waterline working as designed.

---

## Potholes and Sinkholes

A pothole is visible, localised, fixable. You can see it. You patch it before someone drives into it. The road keeps working. Pothole fixes compound — the road gets better over time.

A sinkhole is invisible until it isn't. The surface looks fine. Below it: a cavity, years of slow erosion that nobody saw because nobody was looking at the substrate. Then one morning the road is gone.

Zero-day vulnerabilities are sinkholes. Nobody designed the buffer overflow in OpenSSL. Nobody planned the use-after-free in the browser's JavaScript engine. Nobody intended the integer overflow in the image decoder. The code looked fine on the surface. Below the waterline: a cavity that an attacker found before the defender did.

**Just because you did not design them does not mean you do not have to fix them.**

The road authority that says "we did not design this sinkhole therefore it is not our responsibility" is the organisation that watches a bus fall into it. The sinkhole does not care who designed it. The bus does not care who is responsible. The road must be fixed.

The software organisation that says "we did not write this dependency therefore its vulnerability is not our problem" is the organisation that ships a product with a known sinkhole in the substrate. The attacker does not care who wrote OpenSSL. The customer's data is gone either way.

---

## Every Night at 2am

The browser zero-day fixed silently overnight is sinkhole repair at scale.

The browser vendor found the cavity before the attacker drove into it. The patch was written, tested, staged, and deployed. The update arrived on your machine while you slept. You opened the browser in the morning. The sinkhole was filled. Life went on.

The Gutenberg layer (the JavaScript engine, the rendering pipeline, the network stack, the sandbox) was patched. The Semantic layer (your web applications, your bookmarks, your browsing history, your workflow) never noticed. The waterline held. The improvement arrived for free.

VS Code every week: new features, performance improvements, language server updates. The editor improves. Your code stays where you left it. The cursor is where you left it. The extension still works. The waterline held.

This is Moore's Law as a lived experience — not the abstract doubling of transistors every two years, but the practical consequence: the Gutenberg layer keeps improving, the Semantic layer above the waterline collects the gains for free, and you wake up to a slightly better world without doing anything.

---

## The Maintenance Obligation

"We use open source therefore it is free" misunderstands the economics.

The code is free. The maintenance obligation is not. The sinkhole appears in the free code. The fix requires someone to pay attention, find it, patch it, test it, deploy it. That is not free. It is usually paid by someone else — until it is not.

Heartbleed in OpenSSL, 2014: a mistake in code written in good faith in 2011. Two years of slow erosion below the surface. When the sinkhole appeared it exposed private keys on half the internet's servers. The internet did not design Heartbleed. The internet had to fix it overnight regardless.

The XZ Utils backdoor, 2024: a patient attacker spending two years building trust in a maintainer role, then inserting a sinkhole into a compression library that feeds into SSH on half the world's Linux servers. Caught by a Microsoft engineer who noticed his SSH logins were 500ms slower than expected. A pothole-fixer noticing an anomaly in the Gutenberg layer. The sinkhole was caught before it opened.

The maintenance obligation is not optional. The substrate does not care who wrote it. The road must be kept.

---

## Rust: Turning Sinkholes into Potholes

The class of sinkholes that appear in C and C++ — buffer overflows, use-after-free, double-free, data races — are not random. They are a specific category of memory safety failure that the C language permits by design. The substrate can form these cavities because C gives you the tools to form them and trusts you not to.

Rust makes the substrate incapable of forming that category of cavity. The borrow checker enforces memory safety at compile time. Not by finding the bugs after they appear — by making the code structurally unable to produce them. The sinkhole category is eliminated, not patched.

The Linux kernel allowing Rust for new drivers is the dual-track sinkhole prevention approach. Not rewriting the existing C — that is Schmiel, smudging the floor. Adding new code in a language that cannot form the same class of cavity. The old C code keeps running. The new Rust code cannot produce the buffer overflow that Heartbleed was. The road stays open. The new lanes are safer.

Pothole fixes compound. Sinkhole prevention compounds faster.

---

## Fusion and Mars: Not Pothole Problems

Meanwhile fusion is still thirty years away. Mars is still thirty years away.

Not because the engineers are incompetent. Because fusion and Mars are not pothole problems. They are mountain problems — the obstacles are fully visible, fully understood, and very large.

**Fusion:** the plasma confinement problem is not a sinkhole waiting to be found. It is a known hard problem requiring simultaneous solutions to plasma physics, neutron embrittlement, tritium breeding, and net energy gain at commercial scale. Every layer is fused to every other layer. You cannot solve the confinement problem without knowing the final geometry. You cannot know the final geometry without solving the materials. The whole iceberg must move simultaneously. ITER has been under construction for twenty years to prove the physics. Commercial energy: still thirty years away.

**Mars:** the rocket is fused to the life support is fused to the landing system is fused to the radiation shielding is fused to the two-year mental health of the crew in a tin can. SpaceX is making genuine progress by fixing potholes — reusable first stage, iterative Starship testing, Raptor engine improvements. Each pothole cheap. The rocket is getting better. The destination still requires everything to be perfect simultaneously. Thirty years away.

**Solar and batteries: pothole problems that are being solved:**

Solar panel efficiency: up from 6% in the 1950s to 24% commercial today. Not a breakthrough. Compound pothole fixes in materials science and manufacturing. The physics did not change. The cost of producing the physical artifact fell 90% in ten years.

Battery energy density: doubling roughly every decade. Not fusion. Incremental chemistry compounding.

Wind: taller towers, better blades, smarter controllers. Each improvement small. The aggregate: wind is now the cheapest electricity source in history.

No silver bullet. A thousand cheap moves at the fast layer. The energy transition is happening not because fusion arrived but because the pothole fixes crossed the economic threshold where the Gutenberg layer became cheaper than burning things.

The Gutenberg layer (the manufacturing, the materials, the cost curves) improved on its own schedule. The Semantic layer (abundant clean energy) is the destination. The waterline between them is where the work happens. The potholes are being fixed every night.

---

## Every Morning, Mostly Unchanged

The zero-day is fixed. The editor is slightly better. The solar panel is slightly cheaper. The battery is slightly denser. The browser is slightly safer. The Linux kernel has one more Rust driver that cannot produce a buffer overflow.

None of these individually impressive. All of them compounding.

You did not notice most of the improvements. That is the proof they worked. The best Gutenberg improvement is the one that arrives invisibly and the Semantic layer never knows happened.

The fusion reactor is still thirty years away. The Mars colony is still thirty years away. The sinkhole that will appear in some C code somewhere is being formed right now in code written in good faith by a competent engineer who trusts the language.

The browser will fix it overnight. Life will go on mostly unchanged. The waterline will hold.

Fix the potholes. Fill the sinkholes. Let the Gutenberg layer improve while the Semantic layer sleeps.

Nothing broken. That is all it takes. Compounded over thirty years, it is everything.

Move on. Nothing broken. Most is well.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) on collecting the gains for free, [You Actually Move Faster Without Breaking Things](https://rinie.github.io/2026/07/03/you-actually-move-faster/) on potholes compounding, [Cheap Moves Instead of Silver Bullet](https://rinie.github.io/2026/06/27/cheap-moves-instead-of-silver-bullet/) on why the fast layer has leverage and the slow layer does not, and [The Watershed](https://rinie.github.io/2026/07/02/the-watershed/) on thirty years of the Linux kernel being patched while the applications above it never noticed.*
