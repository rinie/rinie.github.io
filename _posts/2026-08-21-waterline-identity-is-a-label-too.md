---
layout: post
title: "Below the Waterline, Identity Is a Label Too"
date: 2026-08-21
tags: [gutenberg-semantic, zigbee, mqtt, matter, home-assistant, identity, resolver, waterline]
level: technical
description: "Zigbee2MQTT's network map takes minutes to render and tells you almost nothing when it finishes. Matter's Fabric→Node→Endpoint→Cluster tree makes the same mistake in a different font — the producer's internal structure surfaces as the consumer-facing API. There's a second, less obvious waterline underneath the data one: identity is a label too, and conflating a device's raw key with its identity is the same mistake Thunderbird made with folders."
---

Zigbee2MQTT has a network map. It's a force-directed graph — nodes, edges, a yellow dot in the middle where the coordinator sits, lines fanning out to routers, routers fanning out further to end devices. On a network with any real size it takes minutes to render. A [recent write-up of a new Zigbee coordinator](https://tech.scargill.net/sonoff-dongle-plus-cc2674p10-the-new-zigbee-coordinator/) describes exactly this: requesting the map, going for coffee, and ten minutes later getting a picture the author's own words dismiss as *"like that's going to mean anything."* It's the same picture as Obsidian's graph view, and Matter's Fabric → Node → Endpoint → Cluster tree makes the identical mistake in a different font: it exposes the producer's internal structure *as* the consumer-facing API.

Home Assistant doesn't do this. Neither does IKEA's Dirigera. Ask Dirigera for its devices and you get a flat array — id, type, a bag of attributes. No mesh, no routing, no fabric. The topology is real and it's genuinely graph-shaped — you cannot flatten "which router repeats for which sensor" without lying about the network — but the topology isn't the *API*. It's infrastructure below the waterline. What surfaces above it is flat.

That's the familiar half of the Def-Push/Use-Pull story: producers push whatever shape their own concerns force on them (a mesh needs a graph to route), consumers pull a flat, meaning-free waist regardless. Diamond to hourglass. Nothing new there.

The part worth adding: **identity has its own waterline, and it doesn't line up with the data's.**

---

## The Mailbox That Isn't One

Thunderbird stores mail as a physical folder tree — a message lives in exactly one `.mbox` file, in exactly one folder, because that's how the underlying storage works. Gmail doesn't have that constraint and doesn't pretend to: a message carries labels, plural, and "which folder is it really in" isn't a question the model even asks. The folder tree is Def-Push — a *storage* concern leaking into the *experience*. Labels are Use-Pull — cheap to add, cheap to remove, no ownership dispute, because nothing was ever moved anywhere.

Naming a device has exactly the same fork, and it's easy to build the Thunderbird version by accident. A MAC address, an IP, an IEEE 802.15.4 address, an mDNS hostname — each of these looks like a stable key, so the obvious data structure is a `Map<key, canonicalDevice>`. One key, one owner, forever. That's a folder. It's wrong for the same reason Thunderbird's folder is wrong: the "stable" key isn't actually stable. BLE MACs rotate by policy. DHCP leases get reassigned. A board reflashed with its default hostname collides with the next one off the same image. The alias was never a fact about the device — it was an *observation*, timestamped, and observations expire.

---

## Five Names for One Phone

Android Auto will offer you a phone under whichever name Samsung, the carrier, the Bluetooth stack, and Android itself each independently decided to call it — never the one you actually typed in, "Rinie's A56." That's not a bug in any one of those five systems. It's what happens when nothing is designated as the single place identity gets resolved *before* display. Each subsystem is confidently Def-Push about its own name and nothing arbitrates.

The fix isn't picking a winner among the five. It's the Gmail move: introduce a layer whose only job is "given a raw observation, what does *this asker* call it right now" — checked first, not consulted as a tiebreaker after the native names have already rendered. The five native names don't go away; they just stop being authoritative.

---

## The Universal Broker Is the Same Mistake in a Different Hat

MQTT looks like it dodges all of this. There's no cluster tree, no fabric hierarchy — just a broker and a string. But the moment Home Assistant, Node-RED, and a hand-written script all have to agree on the exact same topic path to refer to the same device, that topic string has become a canonical tree in every way that matters, just spelled with slashes instead of an XML schema. `zigbee2mqtt/livingroom_sensor/temperature` is a folder path wearing a pub/sub costume. Whichever component publishes first effectively wins the name, and every other consumer either adopts that exact string or maintains its own private mapping — which is the five-names-for-one-phone problem, recreated at the topic-namespace layer, by a protocol whose whole design goal was decoupling.

Scargill's own write-up shows the same tree asserting itself from an unexpected angle: devices marked as routers-only don't appear in Zigbee2MQTT's Home view at all — only in the Devices tab, for reasons the author himself couldn't find documented anywhere. The device joined. It has an identity. It's real, addressable, and functioning. But the UI's own internal category — router versus end device, a Gutenberg-layer fact about network role — determined whether a human asking "did my new device join?" could find it at all. That's identity being filtered through a producer-side classification scheme that has nothing to do with the question actually being asked.

Even Zigbee's own vocabulary carries the same trap one layer up: what Zigbee calls a *coordinator* is what most people would call the network's central router, and what Zigbee calls a *router* is closer to a repeater. Same words, two incompatible meanings, depending on which framework you're standing inside when you use them — the protocol-level version of Android Auto's five phones, except here it's one word carrying two different senses instead of five words for one thing.

A universal broker was supposed to be the fix for a universal tree. It's the same mistake, dressed as its own cure, because introducing one central routing point never actually removes the canonical-namespace problem — it just relocates the argument about whose name wins from a schema committee to whichever service happens to publish first.

---

## What Actually Goes Below This Particular Waterline

So there are two waterlines stacked on top of each other, and they carry different things across:

- **The data waterline** — mesh topology, cluster trees, message-header nesting stay below; flat entity/attribute rows surface above. This is the diamond → hourglass move already covered here.
- **The identity waterline** — raw keys (MAC, IP, hostname, IEEE address, serial number) stay below, each one just an observation with a validity window; a resolved, asker-aware label surfaces above. Multiple live aliases for one entity, and occasionally multiple entities briefly claiming the same alias, are both normal — surfaced, not silently collapsed into a guess.

Conflating the two is the trap. It's tempting to let the entity id *be* the MAC address, because for a lot of devices, most of the time, it works. Then a device gets reflashed, or two BLE peripherals randomise into a collision, or a Zigbee device also happens to advertise BLE during provisioning and now has a foot in two address spaces — and the "stable" key wasn't. The fix isn't a smarter key. It's admitting the key was never the identity, just today's cheapest way to point at it — the same admission Gmail made about folders and Android Auto still hasn't made about phone names.

[Worse ages better than perfect](https://rinie.github.io/2026/06/28/worse-ages-better-than-perfect/), again: a resolver that says "ambiguous, here are two candidates, pick one" ages fine — you get more evidence over time and the ambiguity resolves itself or stays honestly unresolved. A resolver that silently commits to whichever MAC-lookup answer came back first doesn't age at all. It just accumulates wrong answers that look confident.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [UUIDs Are Not Names](https://rinie.github.io/2026/05/27/uuids-are-not-names/) on Gutenberg identifiers leaking into the semantic layer, [Ambiguity Is Not a Bug](https://rinie.github.io/2026/06/22/ambiguity-is-not-a-bug/) on resolvers that should surface uncertainty rather than hide it, and [Marvin Remembers the Serial Number of Every Car](https://rinie.github.io/2026/06/30/marvin-remembers-serial-number/) on the plate versus the VIN.*

Source: [Sonoff Dongle Plus CC2674P10 – The New Zigbee Coordinator](https://tech.scargill.net/sonoff-dongle-plus-cc2674p10-the-new-zigbee-coordinator/), Peter Scargill, Scargill's Tech Blog, August 4, 2026.
