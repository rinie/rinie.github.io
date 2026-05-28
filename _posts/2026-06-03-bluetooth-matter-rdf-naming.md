---
layout: post
title: "Your Lights Don't Know Your Name: Bluetooth, Matter, and the Semantic Web That Wasn't"
date: 2026-06-03
category: "Systems & Infrastructure"
tags: [naming, bluetooth, matter, iot, semantic-web, rdf]
depth: technical
---

You bought a smart bulb. You followed the instructions. You scanned the QR code, entered the commissioning code, selected your Wi-Fi network, waited for the device to join the fabric, and assigned it to a room. Twenty minutes later your light works. You ask your phone to turn it on. It turns on.

Your light does not know it lives in your kitchen. It does not know your name. It knows its MAC address, its fabric ID, its Thread node identifier, and its Matter endpoint cluster configuration. These are excellent Gutenberg identifiers. They are useless to you.

This is not a smart home problem. It is a boundary problem — and it has been failing users at every layer of the stack, from the radio protocol to the academic vision of a machine-readable web.

---

## Bluetooth: The Radio Engineer's Interface

Bluetooth devices are identified by MAC addresses — `AA:BB:CC:DD:EE:FF` — pure Gutenberg identifiers. Globally unique, content-free, meaningful to the radio stack and to nothing else. The semantic layer the user needs is simple: "Rinie's headphones", "Kitchen speaker", "Car hands-free". The boundary between the MAC address and the human name should be owned by the OS pairing system. It almost never is.

The boundary leaks at every point:

**Naming is manufacturer-assigned, not user-assigned.** Devices advertise themselves as `LE-Bose QC45` or `WH-1000XM5` — the manufacturer's model name. You own two Sony headphones. Both appear as `WH-1000XM5`. The Gutenberg identifiers (different MACs) are distinct. The semantic names are identical. The OS gives you no reliable way to distinguish them until you rename both manually — outsourcing the naming problem to the user.

**Protocol variants surface as separate devices.** Bluetooth Classic and BLE are different Gutenberg transport protocols. The same physical headphones may appear twice in your device list with subtly different names — one for audio, one for control. The user's semantic model is one device. The Gutenberg reality is two protocol stacks. The boundary did not hold.

**The pairing PIN is a UUID handed to the user.** `0000`. `1234`. `123456`. These are authentication tokens — Gutenberg credentials — that the user must physically handle, remember, and type. The semantic action the user wants to perform is "connect this device to my phone." The Gutenberg mechanism requires them to participate in a shared-secret protocol that belongs below the boundary.

**AirPods as the counter-example.** Apple owns the full stack — the chip (H1/H2), the OS, the pairing protocol (Apple's proprietary extension over BLE), the cloud (iCloud). They hid the MAC address completely. Pairing is invisible — open the case near an iPhone and a prompt appears with the device name and a single tap to connect. "Rinie's AirPods Pro" appears on every Apple device automatically, synced through iCloud. The Gutenberg layer is completely hidden. The semantic layer is instant, personal, and correct.

The price is lock-in. AirPods work this way only within Apple's ecosystem because Apple solved the boundary problem by owning both sides of it. The Gutenberg/Semantic separation is clean precisely because no committee was involved.

---

## Matter: The Committee That Promised to Fix It

Matter (formerly Project CHIP) was announced as the solution to smart home fragmentation. Every device speaking a different protocol — Zigbee, Z-Wave, Wi-Fi, Bluetooth — every ecosystem requiring its own hub, every setup flow incompatible with every other. Matter would be the unified semantic layer: one standard, any device, any brand, any ecosystem.

The promise was real. The execution exposed what happens when a boundary is designed by a committee of competing interests rather than by a user trying to turn on a light.

**Commissioning codes are UUIDs handed to the user.** Every Matter device has a setup payload — a QR code or an 11-digit numeric code printed on the device or its packaging. The user must scan or type this code to add the device to their network. This is the Bluetooth PIN problem formalised into a standard. The semantic action is "add this device to my home." The Gutenberg mechanism requires the user to handle a commissioning credential that belongs below the boundary.

**Fabrics leak into the user experience.** A Matter fabric is a logical network — a Gutenberg grouping of devices sharing a trust domain. Multi-admin — the ability to use the same device in Apple Home, Google Home, and Amazon Alexa simultaneously — requires the user to commission the device into multiple fabrics separately. "Which fabric is this device on?" is a question that belongs in a radio engineer's design document. It does not belong in a consumer setup flow.

**Thread border routers are Gutenberg infrastructure that users must manage.** Thread is the low-power mesh protocol that many Matter devices use for communication. It requires a Thread border router — a device that bridges the Thread mesh to the IP network. The Apple HomePod, the Google Nest Hub, the Amazon Echo — each can be a Thread border router. Whether your device connects reliably depends on which border routers are in range and whether their firmware is current. This is Gutenberg infrastructure. The user experiences it as "my light randomly stops responding."

**The consortium's interests are visible in the design.** Matter was created by Apple, Google, Amazon, Samsung, and the Connectivity Standards Alliance. Each member had an incentive to keep their semantic layer primary — their app, their voice assistant, their ecosystem. The result is a standard that achieves Gutenberg interoperability (devices can talk to each other) without achieving Semantic interoperability (the user's experience is unified). You can commission your light into three ecosystems. You still have three apps.

---

## RDF: The Academic Version of the Same Problem

While consumer electronics engineers were building Bluetooth and smart home engineers were building Matter, the academic and enterprise world was building its own solution to the same naming problem — and failing in the same way for the same reason.

The Resource Description Framework (RDF) is the W3C standard for representing knowledge as a graph. Its fundamental unit is the triple: subject, predicate, object. Every entity in the world gets a URI — a Gutenberg identifier with a semantic namespace. Relationships between entities are explicit predicates. The result is a machine-readable graph of meaning:

```
<https://rinie.github.io/me> <foaf:knows> <mailto:mrs.kervel@example.com>
<mailto:mrs.kervel@example.com> <foaf:name> "Mrs Kervel"
<tel:+31461234567> <schema:contactFor> <mailto:mrs.kervel@example.com>
```

This is the formal solution to the Mum/Mrs Kervel problem from the [UUID post](https://rinie.github.io/2026/05/27/uuids-are-not-names/). The same phone number can have multiple semantic names in different contexts because the graph represents relationships explicitly rather than assuming a one-to-one mapping. Every entity is a URI; every relationship is a predicate; the context is captured in the graph structure.

Technically correct. Elegant in theory. Almost never used outside academia and enterprise data integration.

**Why RDF failed the user:**

The Gutenberg identifiers (URIs) leaked into every triple. Writing RDF requires choosing, minting, or looking up URIs for every entity you want to describe. `<https://rinie.github.io/me>` is a Gutenberg address. The user who wants to say "I know Mrs Kervel" does not think in URIs. They think in names. The boundary between the URI and the name is the same boundary that Bluetooth fails to maintain — and RDF pushes it onto the author of every triple.

The tooling was designed for ontology engineers, not for people who want their contacts to show the right name. SPARQL — the query language for RDF graphs — requires the user to know the URI of the thing they are querying. Turtle, N-Triples, JSON-LD — the serialisation formats are machine-friendly and human-hostile. The W3C working groups that designed these formats were Def-Push tribes: technically rigorous, oriented toward their own use cases, uninterested in the friction they were creating for everyone else.

**What RDF got right:**

The triple model is genuinely the correct semantic structure for the naming problem. Subject, predicate, object — with URIs as stable Gutenberg identifiers beneath human-readable labels — is how you represent the fact that `+31 46 123 4567` is "Mum" to you, "Mrs Kervel" to a colleague, and "Rinie's mother" to a grandchild. The data model is right. The user interface was never built.

Schema.org — Google's practical application of RDF concepts to web markup — is the most successful version of the idea precisely because it hid the URI machinery. You add `<script type="application/ld+json">` to your HTML and describe your content in a JSON structure. The URIs are there internally (`schema:name`, `schema:address`) but the user-facing interface is a JSON object with readable keys. The Gutenberg layer (the URI identifiers) is present but below the boundary. The semantic layer (the JSON structure) is what the developer touches.

---

## The Common Failure Mode

Bluetooth, Matter, and RDF failed the user in the same way:

**The designers solved the Gutenberg interoperability problem** — devices can discover each other, triples can be merged across graphs, devices can join fabrics. The physical layer works. The protocol is correct.

**They left the Semantic layer as an exercise for the user.** The naming, the context, the human-readable identity — these were assumed to be solvable later, by someone else, in an application layer that the standard did not define. The user ended up maintaining the mapping themselves: renaming Bluetooth devices manually, commissioning Matter devices into each ecosystem separately, minting URIs for every entity they wanted to describe.

**The Def-Push tribe designed for itself.** Radio engineers designed Bluetooth for radio engineers. W3C working groups designed RDF for ontology engineers. The Matter consortium designed Matter for ecosystem owners. Each tribe solved the problem they had. The user's problem — turn on the light, find the contact, describe the relationship — was a different problem that the Def never acknowledged.

---

## What the Boundary Looks Like When It Works

The pattern of success is consistent across every case in this series:

- **AirPods**: Apple owns both sides of the boundary, hides the MAC address, names the device after the product
- **DNS**: the boundary is in one system (the resolver), the human uses the name, the network uses the address
- **Git refs**: the boundary is in one system (the ref store), the human uses `main`, the content addressing uses the SHA
- **Schema.org**: the URI machinery is below the boundary, the developer touches readable JSON keys

In every case a single system owns the boundary and maintains the mapping. The user never handles a Gutenberg identifier unless they explicitly ask for one.

The smart home that works like AirPods — where adding a device is as simple as bringing it near your phone, it appears with a sensible name, and it works in every app without further commissioning — requires someone to own both sides of the boundary. Matter cannot deliver this because it was designed by a committee of competitors each defending their semantic layer. The Gutenberg interoperability is real. The semantic unification requires a single owner, and no single owner was acceptable to the consortium.

Until then: pairing code `0000`, fabric not found, border router unreachable, your light still doesn't know your name.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [UUIDs Are Not Names](https://rinie.github.io/2026/05/27/uuids-are-not-names/) on Gutenberg identifiers leaking into the semantic layer, and [Why IPv6's Def-Push Failed](https://rinie.github.io/2026/05/28/why-ipv6-def-push-failed/) on load-bearing boundaries that committees tried to remove.*
