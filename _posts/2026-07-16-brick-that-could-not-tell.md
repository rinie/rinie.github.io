---
layout: post
title: "The Brick That Could Not Tell If It Was in the Living Room or the Kitchen"
date: 2026-07-16
tags: [gutenberg-semantic, microkernel, mach, plan9, android, dbus, seams, o1]
level: technical
description: "The microkernel brick did not just say the room sux. It genuinely could not tell which room it was in — and had to ask at runtime, paying a context switch for the question, on every single operation. Why should the brick care? It should not. The room is above the waterline. The brick holds the wall up. That is all."
---

The microkernel dream was architecturally correct and practically catastrophic. Every OS service in its own process. Clean semantic separation. Each component replaceable independently. The floor plan was beautiful. The bricks refused to build it at acceptable cost.

Not because the rooms were wrong. Because the bricks had to ask which room they were in on every operation — and asking costs a context switch.

---

## The O(1) Lesson the Microkernel Ignored

The page boundary is the Gutenberg unit. The context switch is the cost of crossing a process boundary. The flat bytestream over pages is O(1) per page — the libc buffer reads one page from the kernel per syscall, the application processes characters at CPU speed, no boundary crossing per character.

The Mach microkernel put every OS service in its own process. The filesystem server. The network stack. The device drivers. Each one correct. Each one isolated. Each one requiring a full context switch on every IPC message.

The flat bytestream over pages is O(1) per page. The Mach IPC between microkernel components is O(n) per message, where n is the number of component boundaries crossed. A file read that crosses the filesystem server, the block device driver, and the cache manager crosses three context switch boundaries before a byte reaches the application.

The semantic decomposition was correct. The Gutenberg cost was catastrophic.

---

## The Brick That Could Not Tell

The microkernel brick did not just say the room sux. It genuinely could not tell which room it was in.

The Mach IPC message arrives at the filesystem server. Is this request from the application layer or from the network stack? Is this a read for a local file or a remote mount? The server has to ask — another IPC crossing, another context switch — to find out. The brick that knows it is a living room brick is bad (semantic label fused to Gutenberg artifact). The brick that has to ask which room it is in at runtime is worse — asking costs a context switch on every single operation.

The monolithic kernel brick knows nothing about rooms. It is just a brick, positioned in the wall, doing its structural job. The question of which room it is in is above the waterline where it belongs. The kernel does not ask. The application above the libc boundary decides what the bytes mean.

**Why should the brick care which room it is in?**

It should not. The room is above the waterline. The brick is below it. The brick's job is to hold the wall up. That is all.

---

## The Plan Carved in Stone

The microkernel performance measurements were not 10% waste. They were an order of magnitude regression. The Check was unambiguous: the seams are in the wrong places. The correct Act: pivot. Accept that the Plan was a leap of faith. Redraw the floor plan with one seam — kernel versus user space — not dozens of IPC crossings between services.

But the Plan was carved in stone. The papers were published. The architecture was named. The PhD theses were written. The vision had become identity. Rethinking the Plan was not a 10% seam correction — it was an admission that the entire intellectual investment was in the wrong direction.

So instead: fix the Do. Mount BSD monolithically inside a single Mach task. The entire BSD subsystem — filesystem, network stack, POSIX layer — running with no IPC boundaries between its components. The semantic modularity abandoned. The Gutenberg performance recovered. The architecture diagrams still show the beautiful microkernel. The runtime ignores them.

Deming's Check should validate both Do and Plan. Here it validated only the Do — "did we implement the vision" — and never asked "was the vision correct." The waste that should have produced a pivot produced a kludge instead. Persevere stubbornly rather than pivot.

GNU Hurd persevered for thirty-five years. No kludge. No users. The Plan still pure. The vision still beautiful. The bricklayers never finished the house because the cost of crossing the seams exceeded the value of the modularity. Linux posted to comp.os.minix in 1991 and shipped. Hurd is still mostly theoretical.

---

## DBus: The Same Mistake at the Desktop Layer

The Linux desktop repeated the microkernel mistake with DBus. Every inter-process communication between desktop components — notification daemon, network manager, audio system, session manager — goes through DBus. Every message a context switch. Every desktop action a round trip through the message bus.

The semantic architecture is clean. The Gutenberg performance is terrible. DBus is Mach IPC with a different name and the same O(n) cost per message. The notification that takes 50 milliseconds after you plug in a USB device is DBus crossing six component boundaries. The brick asking which room it is in, six times, for every notification.

The Linux desktop tribal Def drew the seams at every component boundary and called it modularity. The brick did not care about the modularity. The brick cared about the context switch.

---

## Plan 9: The Seam in the Right Place

Plan 9 from Bell Labs got closer to the correct answer than Mach did — and still did not win.

Plan 9's insight: if everything is a file in Unix, make everything a file across the network too. The 9P protocol — a clean, simple file protocol that lets any resource anywhere appear as a local file. The seam is at the file interface, not at the IPC message boundary. The namespace is per-process. Remote files appear local. The application does not know or care where the bytes come from.

The 9P message is O(1) parseable. Fixed structure. No typed port rights embedded in the message body. No capability semantics requiring the brick to ask which room it is in. Just: read this file, write that file, here is the byte range. The postman reads the address. The brick holds the wall up.

Plan 9 did not win. Not because the seams were wrong — they were more correct than Mach. Because the user interface was wrong. The terminal was beautiful to the Bell Labs engineer. The Bell Labs engineer was not the 90%. The window system was clean, minimal, correct, and completely alien to anyone who had used Windows or Mac. The 90% who would have validated or corrected the UI were never in the room. No Use-Pull. No evolution. The seed was correct. The forest never grew.

The 9P protocol survived anyway. It lives in the Linux kernel's Plan 9 filesystem support. In WSL2, which uses a 9P-derived protocol to share files between Windows and Linux. In Inferno. The seeds travelled. The forest grew elsewhere.

---

## Android: The Right Granularity

Android and Chrome OS did what Plan 9 could not — took the correct parts and put a user interface on top that the 90% could actually use.

Android's modularity advances learned from the microkernel lesson. Not fine-grained IPC between every component — coarse-grained modularity at the right layer:

**Binder IPC** — used sparingly, at the application/system service boundary, not between every OS component. The context switch cost is justified because the boundary is real and coarse.

**HAL (Hardware Abstraction Layer)** — the seam between the kernel and hardware-specific code. Clean interface. Samsung's HAL, Qualcomm's HAL, Google's HAL — all behind the same interface. The Gutenberg layer (the hardware) replaceable without touching the Semantic layer (the Android framework).

**Project Treble** — the explicit separation of the Android framework from vendor-specific code. Before Treble: updates required hardware manufacturers to update their proprietary HAL simultaneously — the microkernel problem in supply chain form. After Treble: Google updates the framework, the vendor HAL stays stable, security patches ship without waiting for the manufacturer. The waterline held.

**Project Mainline** — updatable modules through the Play Store without a full OS update. Critical security fixes at update pace. Stable OS at release pace. The dual-track in production.

Each one: a coarse-grained seam at the right layer. The Gutenberg cost of each crossing justified by the modularity benefit. Not microkernel fine-grained. Not monolithic fused. The brick does not ask which room it is in. The seam is clean. The components move independently.

Chrome OS completes the arc. Everything is a web page. The file system is mostly hidden. Google Drive appears as a local folder — the 9P insight applied to consumer computing, the distributed file abstraction at the user level. Not as elegant as Plan 9. Ships in schools worldwide. The 90% got a simple, maintainable, secure computer. Plan 9 gave them a terminal prompt.

**One UI: mostly harmless. Bixby: the warning sign in miniature.**

Samsung's One UI is the manufacturer doing what manufacturers do — putting design opinions on top of the open platform. Different icon colours. Home screen layout opinions. Bixby pre-installed on a hardware button. Mostly harmless: the core Android functionality works, the HAL and framework seams are unaffected, the user can install Google apps alongside Samsung apps.

But Bixby is the precise warning sign. Samsung built their own assistant because the platform was open enough to replace Google Assistant with a Samsung-controlled alternative — the seam was clean enough to slide Bixby in. The Use signal arrived clearly: nobody uses Bixby, everyone routes around it. Samsung ignored it for years. The Def-Push of the hardware manufacturer, the sticky note nobody filed because the workaround became standard practice.

The Galaxy AI features make the warning more precise. The user sees "AI assistant." The implementation is somewhere in a cloud they did not choose, running a model they did not select — Gemini, Samsung's hosted services, or whoever Samsung partnered with this quarter. The seam between "phone AI" and "which model is actually running" is invisible. The opaque redirect. The barrier reads the plate. The VIN is Somebody Else's Problem.

The reason One UI stays mostly harmless is Pixel. Without Pixel as the reference device with demonstrably clean seams, Samsung's drift would have no counterweight. With Pixel, the manufacturer who goes too far with their overlay loses users to the clean experience. Competition keeps the seam honest. Remove the competition and the crapware multiplies. The Bixby button becomes un-remappable. The A/B test between assistants becomes "Bixby only."

The EU keeps the competition alive. The open source core keeps the fork possible. Three forces holding the seam open while the IPO clock ticks.

Mostly harmless. For now. The brick does not yet need to know which room it is in. Keep watching.

---

## The Seam in the Right Place

The microkernel put seams everywhere and discovered the O(1) cost at every crossing. Plan 9 put the seam at the file interface and got it right — but never reached the users who would have validated it. Android put the seam at the HAL boundary and the framework boundary and reached two billion users who validated it continuously.

The brick does not care which room it is in. The brick holds the wall up. The room is above the waterline. The seam is the architect's responsibility. The waste is the measure.

Below 10% waste: the seam is in the right place.
Above 10%: move the seam.
Bolt BSD on top and call it architecture: the Plan was carved in stone.

The brick that could not tell if it was in the living room or the kitchen paid a context switch to find out on every operation. The brick that does not need to know — the monolithic kernel brick, the Android HAL brick, the 9P file brick — just does its structural job.

Why should the brick care? It should not. The room is above the waterline.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [More Than 10% Waste? That Was a Leap of Faith.](https://rinie.github.io/2026/07/15/more-than-10-percent-waste/) on the PDCA loop and the Plan carved in stone, [The Billy With Opinions: Linux, Ubuntu, and the Waterline That Keeps Moving](https://rinie.github.io/2026/07/05/billy-with-opinions/) on the Linux desktop tribal seams, [Knowing Your Craft](https://rinie.github.io/2026/07/14/knowing-your-craft/) on the brick that knows its domain, and [There Are No Living Room Bricks](https://rinie.github.io/2026/07/17/living-room-bricks/) on why the Gutenberg unit should not know its Semantic context.*
