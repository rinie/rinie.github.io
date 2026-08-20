---
layout: post
title: "512 to 4K, and Now Android Goes to 16K"
date: 2026-08-28
tags: [gutenberg-semantic, a4, disk-sectors, advanced-format, 512e, cpu-word-size, memory-paging, quantization, resolver]
level: technical
description: "A4 and Letter waste part of the last page so every page can be uniformly addressed. Disk sectors made the identical trade, then doubled eightfold behind a resolver. Memory paging ran the same arc: 512 bytes on DEC systems, then 4KB as the x86 default for forty years — and since November 1, 2025, Android requires 16KB pages for new Google Play apps, with a compatibility mode carrying the old assumption until developers catch up. Same doubling, three unrelated layers, one underlying reason."
---

A fixed page size wastes space. The last page of a document is rarely exactly full, and that waste is the honest cost of uniform addressability — [any layer above the page can treat every page as interchangeable, filed, printed, or cached by number, precisely because none of them vary in size](https://rinie.github.io/2026/08/27/malloc-pages-versus-paragraphs/). A page that's precisely as big as its content can't be addressed the same way twice.

That explains why the size has to be fixed. It doesn't explain something disk sectors prove happened anyway: the fixed size itself changed, generationally, without anyone above it noticing.

---

## Fifty-Four Years, One Number

512 bytes was the hard drive sector size from 1956 until roughly 2010 — over half a century as the fixed physical unit every disk controller, filesystem, and piece of software above it was built to assume. Then areal density kept climbing, and the fixed overhead every sector pays for gaps, sync marks, and error correction started eating a larger share of a shrinking sector. In March 2010, the storage industry's IDEMA consortium completed a new standard: eight legacy 512-byte sectors combined into a single 4096-byte sector, cutting that fixed overhead by roughly 10% and buying real capacity headroom the old size had run out of.

That's a real, physical Gutenberg-layer number — the page's actual size — doubling eightfold. It's not the kind of change a resolver can hide by definition; the bytes on the platter genuinely rearranged. What makes it interesting is that almost nothing built on top of that number had to change at all.

---

## The Bridge Has a Name

The mechanism is called 512e — 512-byte emulation. A drive using it physically stores data in native 4096-byte sectors, but presents 512-byte logical blocks to anything still asking for them, translating underneath. Writing a single 512-byte logical block means reading the entire 4K physical sector, modifying the requested 512 bytes, and writing the whole sector back — a real, measurable tax, paid exactly at the seam between the old assumption and the new physical reality, and nowhere else. That's the disk-world version of the wasted last page: not space this time, but I/O work, spent specifically to let two quantization generations coexist without either side needing to know about the other.

Windows 8, in 2012, was the first mainstream OS to support 4Kn — native 4096-byte addressing, no emulation, no read-modify-write tax — once enough of the software ecosystem had caught up to make the bridge unnecessary. That's the same two-stage pattern this series keeps finding at every layer: [ship the bridge first](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/), drop it once nobody still needs the old assumption.

---

## And Now Android Goes to 16K

Disk sectors aren't a one-off. The same doubling shows up at every layer that has a fixed quantization unit, and it's worth naming as a pattern rather than a coincidence: as the technology underneath a fixed unit gets denser, the unit's fixed overhead — headers, error correction, table entries — becomes a proportionally worse tax, and the only way to pay it down is to make the unit bigger. Moore's Law doesn't just make things faster. It makes the *quantization grain itself* grow, because a fixed unit's overhead only ever gets more wasteful relative to the capacity around it, never less.

Processor word size ran the identical arc: 8-bit in the 1970s, 16-bit by 1978 with the Intel 8086, 32-bit mainstream by 1985 with the 80386, 64-bit mainstream in the early 2000s. Each step didn't just add capability — it doubled the natural unit a processor moves and addresses in one operation, for the same reason sectors doubled: the old unit's overhead, in this case address space and per-instruction throughput, had become the binding constraint.

Memory paging shows the exact same arc happening a second time, independently, at a different layer, and it isn't finished. DEC's systems in the mid-1970s used a 512-byte page. x86 settled on 4KB as the default sometime in the mid-1980s and kept it there for roughly forty years — long enough that mmap, guard pages, and linker boundaries all quietly hard-coded the assumption that a page is exactly 4,096 bytes. Android is renegotiating that assumption right now, on a public, dated schedule: Google added 16KB page support to Android 15 in August 2024, first as a developer toggle on the Pixel 8 and 8 Pro, then made it mandatory — since November 1, 2025, every new app and update submitted to Google Play targeting Android 15+ must support 16KB pages on 64-bit devices. Google's own benchmarks for the switch: up to 30% faster app launch under memory pressure, roughly 8% faster system boot, measurable battery gains — the same fixed-overhead argument as Advanced Format's 10% capacity recovery, paid out as speed instead of storage.

512 to 4096 to 16384. The same three-step doubling as disk sectors, the same reason underneath it, and the same resolver pattern making the transition survivable: Android ships a compatibility mode that lets 4KB-only native code keep running on 16KB devices while developers catch up, exactly the role 512e played for disk sectors — a bridge that exists specifically so the old assumption doesn't have to break the moment the new one ships.

---

## The Fixed Unit Isn't Eternally Fixed

A4 and Letter have held their exact dimensions since long before any of this — no equivalent "A5-emulation" layer has ever been needed, because paper's fixed cost (a machine, a person, an actual physical sheet) has never compounded the way silicon has. Disk sectors show what happens when the underlying technology *does* compound: the fixed unit that made addressing possible in the first place turns out to be upgradeable too, generation after generation, as long as something is willing to sit at the seam and translate.

The lesson isn't that pages should be fixed. It's narrower than that: a fixed quantization unit buys uniform addressability, and *that* unit is itself just another Gutenberg-layer fact — provisional, improvable, free to double when the physics underneath it finally justifies the change, provided someone builds the emulation layer that makes the transition invisible to everything built on the old assumption.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [malloc() Is a Paragraph. The Page Is Someone Else's Problem.](https://rinie.github.io/2026/08/27/malloc-pages-versus-paragraphs/) on keeping the interface finer than the resource it addresses, and [It Is Always DNS](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/) on shipping the bridge before dropping it.*

Sources: [Advanced Format](https://en.wikipedia.org/wiki/Advanced_Format), IDEMA Long Data Sector Committee standard completed March 2010; Advanced Format Technology Brief, Western Digital, March 2014; [Virtual Memory Systems Should Use Larger Pages](https://u.cs.biu.ac.il/~wisemay/page-size.pdf), on DEC's 512-byte page size and the x86 4KB standard; [Adding 16 KB Page Size to Android](https://android-developers.googleblog.com/2024/08/adding-16-kb-page-size-to-android.html), Android Developers Blog, August 2024; [Support 16 KB page sizes](https://developer.android.com/guide/practices/page-sizes), Android Developers, on the November 1, 2025 Google Play requirement and compatibility mode.
