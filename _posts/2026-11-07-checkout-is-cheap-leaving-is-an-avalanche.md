---
layout: post
title: "Checkout Is Cheap. Leaving Is an Avalanche."
date: 2026-11-07
tags: [gutenberg-semantic, monorepo, dependencies, exit, hiding-the-waterline, borg]
level: technical
description: "A monorepo is a bet that nobody will ever leave. Checkout stays one command however large the tree grows, while leaving is priced by every undeclared import that piled up in between. The same fan-out is the reason to choose a monorepo at all: change a shared library and rebuild every caller in one commit. Declared, the slope is a build graph with a number on it. Undeclared, it's an avalanche. Swapping in a better outside dependency is cheap to execute and costly to start."
---

A monorepo is a bet that nobody will ever leave. For some teams that is a good bet. The trouble starts when it's placed by default, because the price of losing it arrives later, all at once, from a source nobody was watching.

---

## Checkout Is One Command

Cloning the tree is one step however big it gets, and any package can import any other by path with no ceremony. That ease is why people pick monorepos. It's also why nobody has to draw an interface: with every sibling a relative import away, the boundary between two packages never gets forced into the open. It is [hiding the waterline](https://rinie.github.io/2026/06/08/hiding-the-waterline/) at repo scale. The song about the hotel had the asymmetry right, and git supplies the verb.

Leaving is the reverse. `git filter-repo` and `git subtree split` will lift a directory out with its history, but they move files, not meaning. The imports reaching sideways into sibling packages come along unresolved, and the extracted package won't build until someone declares what it had been quietly using all along.

---

## The Avalanche Has a Size

Each undeclared import adds a little mass to the slope. Nothing looks wrong while it accumulates, because nothing depends on it being visible. The load arrives when someone needs to take one package out and finds that everything it reaches inside the tree has to come with it: the transitive closure of in-repo dependencies. It is the shape [the diamond posts](https://rinie.github.io/2026/08/25/the-diamond-shows-up-eventually/) kept finding. Coupling costs nothing to create, and the migration to undo it was never budgeted.

The size can be measured before it matters. A build tool with a declared dependency graph can list what any target pulls in (Bazel's query command does this, for one). For a tree held together by path imports, static analysis gives the same count. That turns "could we leave?" into a test with an answer: publish one package to a registry tomorrow, and count how many other packages come out with it.

---

## The Bet That Pays

Run the same fan-out the other way and it is the reason to choose a monorepo. Change something at the bottom of a shared library and every caller above it is rebuilt and retested in the same commit. There is no version-skew window, no caller on old code against new, no coordinated release. If you want to recompile the iceberg a zillion times a day, the monorepo is the right tool, and [Borg's confidence](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) holds inside its own tree for exactly that reason: every caller is visible and updated atomically.

What separates that bet from the first one is whether the slope is declared. The closure that is an avalanche when it's implicit is a build graph when it's explicit, with visibility rules (Bazel puts a visibility attribute on every target) saying which packages may depend on which. Atomic change, a declared graph, and a number you can print for any package. Nobody is hiding the seams there. The monorepo is paying for atomicity at a price that is written down.

---

## A Better Outside Dependency

Taking in an outside library is the same question aimed the other way, and a monorepo answers it differently from separate repos. Many monorepos adopt a one-version rule: the tree holds a single copy of each outside dependency and every package builds against it. That removes the version conflicts called diamond dependencies, where two packages need incompatible versions of a third. It also makes a swap one atomic change. Replace the library, update every caller in the same commit, and test them all together.

The price is local freedom. A team that finds a better library can't simply adopt it in its own package, because the tree has one version and choosing it is a decision for everyone who builds against it. In separate repos the same team edits its own manifest and ships today, and the cost lands on one package. The monorepo makes the swap cheap to execute and expensive to start. Separate repos do the reverse.

What lowers the price in both is a narrow waist. The fewer files that import the outside library's API directly, the smaller the swap, and a thin adapter in front of it means callers never learn which library answers.

Outside code sitting in your tree has a second way to go wrong. A local patch is one commit away, and each patch turns a vendored library into a fork you maintain, with upgrades becoming merges. Nobody decides to write their own library. The tree just stops being able to take upstream head, which is the avalanche again: an undeclared dependency on your own modified copy. The check is whether you can drop your patches and take the new version today.

The opposite habit costs as much: always shopping for something better. In a one-version tree every swap is a repo-wide change, so a restless search taxes everyone who builds against the old choice, and each adoption adds to the closure that has to come along with any package you later extract. "Better" is also judged against today's constraints, which move, and that is [optimizing for yesterday](https://rinie.github.io/2026/09/08/optimizing-for-yesterday/) in its dependency form. The newest candidate is the least aged one, too, which is what [the seven-day gate](https://rinie.github.io/2026/11/06/the-72-hour-window-measured/) exists to price, and one door for outside code means a new release waits there once for everyone. A swap earns its place when the old library is hurting you, not when a newer one is merely nicer.

---

## Two Conditions

Two things make it a sound bet. Every caller that matters sits inside the tree, and the build enforces the boundaries instead of trusting good intentions. With both, the monorepo is a decision with its exit price printed on the door.

Without them it is the version where leaving was never priced, and that fails [the cheap-Exit test](https://rinie.github.io/2026/09/06/swappable-needs-a-cheap-exit/) in a quiet way. Nobody chose a lock-in. A bill grew where nobody was reading it. Checkout stays cheap either way, and whether leaving turns out to be a number or an avalanche is settled long before anyone wants to leave.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Borg's Arrogance: We Will Just Recompile the Iceberg](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) on atomic change when every caller is visible, [The Diamond Shows Up Eventually](https://rinie.github.io/2026/08/25/the-diamond-shows-up-eventually/) on coupling that surfaces as a migration, and [Swappable Needs a Cheap Exit](https://rinie.github.io/2026/09/06/swappable-needs-a-cheap-exit/) on pricing the exit in advance.*
