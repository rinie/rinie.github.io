---
layout: post
title: "Why IPv6's Def-Push Failed: NAT Was Not a Hack"
date: 2026-05-28
category: "Systems & Infrastructure"
tags: [networking, ipv6, nat, protocols, internet]
depth: technical
---

Every information system has a Def side that publishes definitions and a Use side that consumes them. When the Def side imposes a model on the Use side without waiting for acknowledgement, that is a Def-Push. When it degrades gracefully because the Use side has feedback, that is Use-Pull. Most successful protocols respect this boundary. IPv6 did not — and the story of how NAT went from ugly stopgap to load-bearing architecture is a lesson in why.

---

## The Two-Layer Address Space That Emerged by Accident

IPv4 exhausted its address space. The IETF knew this by the early 1990s and designed IPv6 as the long-term solution. While that work was underway, RFC 1918 (1996) defined private address ranges — 10.x, 172.16.x, 192.168.x — and RFC 1631 introduced NAT as a bridge: one public IP address, many private devices behind it. The intent was explicit: this is temporary, IPv6 will make it unnecessary.

The hack became permanent because it accidentally created something useful — a clean separation between two layers that should have been separate all along:

- **Public (Gutenberg):** globally routable IP address, a license plate, a physical location on the network
- **Private (Semantic):** internal hostname or 192.168.x.x address, meaningful only within a domain

The router at the boundary — the NAT gateway — is a Gutenberg/Semantic separation point. It translates between the global address space and the local one, exactly the way DNS translates between a hostname and an IP, or a postal system translates between a person's name and a delivery address.

Your house has one street address. Your rooms have internal names. Nobody outside needs to know your bedroom is the third door on the left.

---

## What IPv6 Got Wrong

IPv6 was designed around a different assumption: every device deserves a globally unique, routable address. End-to-end addressability was treated as a virtue. NAT was treated as an obstacle.

This is a Def-Push: the address space is defined such that NAT becomes architecturally unnecessary, and the implicit message to the Use side is "restructure your network accordingly." The architects were solving the right Gutenberg problem (address exhaustion) but imposed the wrong Semantic model (global identity per device) on networks that had no need for it.

The problem is that IPv6 conflates two things that should remain separate:

- **Identity** — who or what you are (Semantic)
- **Location** — where packets find you (Gutenberg)

IPv4 + NAT had accidentally separated them. Your laptop's identity is its hostname; its location is a private IP managed internally. The NAT gateway is the boundary where the translation happens. IPv6 tried to collapse this back into one artifact — the globally unique address — in the same way a URL conflates identity with location, which is why link rot exists.

---

## The Emergent Benefits Nobody Planned

NAT was a stopgap. But it delivered unanticipated properties:

**Privacy.** Internal topology is invisible from outside. Nobody can enumerate your devices by scanning your address space.

**Implicit firewall.** Unsolicited inbound connections are dropped by default. The boundary enforces a policy — inside is trusted, outside is not — without requiring explicit configuration.

**Renumbering stability.** Change your ISP, get a new public IP, and your internal addresses are unaffected. Internal hostnames stay valid.

**Human scale.** `192.168.1.1` is a gateway. `192.168.1.53` is your Raspberry Pi. `2001:0db8:85a3:0000:0000:8a2e:0370:7334` is nobody's Raspberry Pi in any meaningful sense.

These are not Gutenberg properties. They are emergent Semantic benefits produced by the existence of the boundary itself. The separation point became valuable *because* it was a separation point.

IPv6 architects saw these as workarounds masking the absence of a proper solution. Network engineers saw them as features worth keeping. That disagreement was never resolved — which is why IPv6 networks routinely bolt on stateful firewalls that recreate the same boundary NAT provided, just without the clean address scoping.

---

## Conway's Law and Chesterton's Fence

Two principles explain why the IPv6 architects made this mistake.

**Chesterton's Fence:** do not remove a fence until you understand why it was built. NAT was the fence. It looked like a hack bolted onto a broken system. Remove it, clean things up, restore the original design intent. But the fence was load-bearing. The benefits listed above emerged *after* NAT was deployed, which meant they were not visible to the architects who decided to route around it.

**Conway's Law** usually runs in one direction: your systems mirror your org structure. But the inverse is also true: good technical architecture respects existing organisational and social boundaries. The public/private split is not a networking concept. It is a property boundary that human societies have maintained for centuries — public road, private home; public DNS, private LAN; public API, private implementation. NAT found that boundary accidentally. IPv6 ignored it deliberately.

The postal analogy is exact. A letter is addressed to a house (public, routable) not to a room (private, internal). The household handles internal routing. Nobody proposes that every room should have its own globally unique postal address so that letters can be delivered without the household acting as a NAT gateway.

---

## The Pattern

This failure recurs:

- **De-Mail** tried to eliminate the Rückschein (Use-Pull feedback) to reduce Behörden workload. The Use side rejected the system because adopting it degraded their legal position. The system punished adoption.
- **IPv6** tried to eliminate NAT (the Gutenberg boundary) to restore end-to-end addressability. The Use side kept NAT because the boundary was doing real work. The system was ignored.

In both cases, a Def-Push tried to remove a boundary that had become load-bearing. In both cases, the Use side found ways to preserve the boundary anyway.

The hack that became the standard solution usually did so because it respected a pre-existing boundary — organisational, legal, physical, or social — that the clean design had overlooked. Before removing a boundary, it is worth asking: what is it separating, and who put it there?

---

*Part of a series on the Gutenberg/Semantic model — the idea that every information system operates on two parallel layers: a physical/positional layer (bytes, addresses, routes) and a logical/meaningful layer (names, identities, content). Clean systems isolate the boundary between them in exactly one place.*
