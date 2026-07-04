---
layout: post
title: "Borg's Arrogance: We Will Just Recompile the Iceberg"
date: 2026-07-30
tags: [gutenberg-semantic, resolver, monorepo, google, intel, arm, aws, azure, android, chrome, linux, rust, gemba]
level: general
description: "Intel owned the iceberg — fab, architecture, roadmap — and out-executed everyone internally until ARM climbed in from outside, from ground Intel never built a resolver for. Borg is the same shape: a monorepo can make breaking changes atomic because every caller is in the same tree. Linux and Chrome can't see their callers, so they freeze APIs and only break them for security holes. Leaders become laggards when the iceberg's quality stops being the test that matters."
---

When you think you own the iceberg, recompiling it every night might seem like a good idea. Intel believed the same thing for decades — own the fab, own the architecture, own the roadmap, out-execute everyone else through sheer internal capability. It worked, until evolution outside the iceberg could no longer be ignored: ARM grew up in phones Intel didn't compete for, then climbed into laptops and servers from the outside while Intel was still optimizing the inside. Leaders become laggards not because they stop being excellent at what they control, but because they cannot move to a better iceberg that had a worse start.

A monorepo is LDAP for code. Everything addressed by position in one tree, owned by one party, no name that survives the directory moving. `import { parse } from '../../../packages/parser/src'` is an iceberg address, not a semantic name — move the package and every import breaks, because the name was never separate from the location to begin with.

This is fine when you own both sides. A frontend and backend that deploy together, version together, with one team responsible for the contract — there's no outside Use to protect, so collapsing the resolver costs nothing. It stops working the moment a stranger needs to depend on one piece without depending on the whole. npm, crates.io, and Maven Central exist because *many independent strangers consuming one package on their own schedule* requires a real resolver: a name stable while the location and version underneath it change.

The question that decides whether a resolver is worth building is simple: does this thing have to interoperate with something you don't control?

---

## Google's Internal Gemba

[Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) means watching the actual Use, where it lives. Google's internal tooling — Borg, Bazel, Google3 — has an enormous, genuinely well-observed Gemba. But the people being watched are other Google engineers. The Use side never extends past the edge of the iceberg, so the resolver never has to hold up under a stranger's weight. Borg was excellent and stayed internal for years; Kubernetes, the externally resolvable version, was built by people outside imitating what they could observe — the same way Maven was built outside Java's `import` statement to fix what Java never resolved.

This isn't company-wide NIH. It's per-product. Search and ads never required interoperating with infrastructure Google doesn't own — the Use side is end users consuming a finished product, not builders depending on a stable seam. The resolver atrophies from disuse, the same way a monorepo's resolver atrophies when nobody outside the repo needs it.

Android and Chrome are the exceptions, and the reason is structural, not cultural. Android ships into hardware Google doesn't manufacture. Chrome has to render the open web built by millions of people Google has no say over. Neither product survives an internal-only iceberg — a browser that can't browse and a phone OS with no third-party apps are both useless. The resolver had to be real because the alternative was a product that didn't work.

---

## AWS and Azure: The Resolver Is the Product

AWS can't go to the Gemba of "people using our internal build system," because that isn't the product. The Gemba is developers at companies AWS has never met, hitting an API for the first time with zero internal context. That forces the resolver — the API surface, the SDK, IAM — to be the entire interface, stable and self-explanatory, because there's no fallback of asking someone on the team. The business model is strangers trusting the seam. AWS cannot afford to let it become an iceberg address.

Azure inherited the same forcing function from a different direction: decades of enterprise customers running Active Directory, Exchange, and Windows Server on infrastructure Microsoft doesn't control. The seam between Azure AD and a customer's own on-prem directory has to resolve correctly, indefinitely, because the customer's iceberg predates Azure and isn't going away. Where AWS built the resolver-first because the product was always meant for strangers, Azure built it because the strangers — decades of enterprise IT — were already there, running infrastructure Microsoft had to interoperate with rather than replace. Different path, same forcing function: the other side was never going to be owned.

## Atomic Change vs Invisible Callers

There's a sharper mechanism underneath all of this. A monorepo doesn't just lack a resolver — it can make breaking changes atomic. If an internal API changes, every caller is in the same tree, checked out together, updated in the same commit. There is no version-skew window, no caller running old code against a new Def, because the change and every consumer of it land at once. The test suite catches the fallout before the commit lands. Breaking changes aren't felt as breaking. They're refactors.

This is MIT-PDCA in its purest form. We will just recompile the iceberg. The whole system is specified, owned, and visible — so verify correctness by rebuilding everything at once and trust the test suite to catch what broke. Borg's internal churn isn't reckless; it's the rational consequence of believing the system is knowable enough to re-derive from scratch on every change. The confidence is earned, locally — inside a tree you fully control, it usually works.

Linux and Chrome sit at the New Jersey-PDCA extreme, and it shows in their deprecation policy. The kernel's "we do not break userspace" rule exists because Linus cannot see or update the millions of compiled binaries depending on a given syscall. There is no atomic commit that updates the kernel and every program that calls it — those programs are out in the world, unreachable, possibly running on machines nobody at Linux HQ will ever touch. The system is not specifiable end to end, so the only safe move is conservatism: assume nothing about the caller, never break what already shipped. The only thing that overrides the freeze is a security hole, because that's the one case where the cost of leaving the interface alone exceeds the cost of breaking it. Otherwise the API is frozen close to permanently — Linux still carries syscalls almost nobody uses, kept alive because deprecating them might break one binary somewhere invisible.

Chrome is the same shape. It can't see or update the millions of websites depending on specific rendering or JS engine behavior, so breaking changes to web platform features go through years-long deprecation — origin trials, console warnings, gradual rollout — precisely because the population of callers is unknowable and unreachable. A security issue compresses that timeline the same way it does for the kernel, for the same reason: the asymmetry flips.

Rust, Node.js, and VS Code land in the middle, and it's worth seeing why they behave more like Linux than like Borg despite being far younger and far more willing to evolve. Each has a registry or marketplace — crates.io, npm, the extension marketplace — full of independently published, independently versioned work built by people the maintainers don't employ and can't coordinate with. Breaking the public API doesn't break one tree you control; it breaks an unknowable number of crates, packages, and extensions simultaneously, none of which you can fix in the same commit. That's why all three lean hard on semver, deprecation warnings, and editions or major-version gates rather than silent breakage — not because the maintainers lack Google's engineering discipline, but because the caller population is structurally invisible to them in exactly the way Linux's is, and exactly the way Borg's internal callers are not.

The mechanism is the resolver argument restated one layer down: when you can see and atomically update every caller, breaking changes cost nothing and happen constantly, and MIT-PDCA's confidence is justified. When the callers are invisible and unreachable, every breaking change is close to permanent — unless a worse cost forces the issue — and New Jersey-PDCA's conservatism is the only thing that survives contact with reality.

---



Not company culture. Not how much NIH a team indulges. The variable is whether the product's core function requires interoperating with something outside the org's control.

If no — the Use side is internal engineers or a finished consumer product — the resolver atrophies, however good the internal version is, however large the internal Gemba. If yes — strangers building on top, hardware you don't manufacture, a directory that predates you — the resolver hardens, because the product fails without it.

The iceberg is not the mistake. Every serious system has one. The mistake is assuming the iceberg's quality substitutes for a resolver, when the actual test was never internal excellence. It was always: can someone outside replace the Def with another implementation, without the Def knowing? If the answer has to be yes, build the resolver before you need it. Waiting until a stranger needs it is how Kubernetes ends up imitating Borg from the outside instead of Borg simply being available.

---

**LDAP** (Lightweight Directory Access Protocol) is the directory system behind Active Directory and most enterprise identity systems — the thing that looks up "who is this user, what groups are they in, what's their email." [Earlier in the series](https://rinie.github.io/2026/07/28/at-sign-layer-boundary/), LDAP showed up as the example of a Gutenberg/Semantic boundary violation: it stores full semantic identity (`rinie.kervel@gmail.com`, complete with org, role, and relationships) inside what should be a lightweight location-lookup layer, the same job DNS does cleanly by resolving a name to an IP and stopping there. That's the same shape as a monorepo — no resolver, because the system was built to be browsed from inside by people who already have the whole graph in front of them, not looked up by a stranger who only needs one fact.

**NIH** (Not Invented Here) is the tendency for an organisation to build its own version of something rather than adopt an existing external one — Google's internal build system instead of adopting something off the shelf, an internal RPC framework instead of an existing one. NIH gets blamed as a cultural failing, arrogance or wasted effort. The argument in this post is narrower: NIH isn't the problem by itself. Borg, Bazel, and Google3 are genuinely excellent NIH, built by people who understood the problem deeply. The cost only shows up when a product built with that same internal-only confidence turns out to need an external Use side after all — and by then the resolver was never built, because nothing inside the iceberg ever needed one.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [com.google.gson Goes Nowhere](https://rinie.github.io/2026/07/29/java-reversed-hierarchy-forgot-resolver/) on modularity as replaceability, [URL, DNS and the @ Sign](https://rinie.github.io/2026/07/28/at-sign-layer-boundary/) on keeping the global layer small and externally resolvable, and [Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on watching the Use signal where it actually lives.*
