---
layout: post
title: "Should I Click? Trusting the Letter by the Envelope"
date: 2026-07-10
tags: [gutenberg-semantic, urls, trust, resolver, envelope, enshittification]
level: general
description: "The readable URL is the envelope with an address. The opaque redirect is the envelope with no address. You are not being superficial by not clicking — you are using the trust infrastructure correctly. The platform removed the envelope on purpose. Your hesitation is the system working as designed."
---

Here is a link:

`https://c.gle/AOExmq350wTz2t1IHYmY416pxvXlKpH28uYNlETx0ZU6ulX8FvCS2TjkUF9A91rhFslZr_XtO9ip-CdF9ejqAWN4MPT8CnHBYKfb7ttJdejW3QuVUpMG4F4nMU0UUgmGptBBkkjuB92l3-OVM8bq1znCUUCdAVd6VYMRgThHCbAl09exmJSiMhBxB1GOTv0ex3ePBPk9yFc9ORUmqpoCG7UHfeBG3SV4JCj34HWV3gJl`

Should you click it?

You cannot tell. That is the point.

---

## The Envelope Has an Address

The old saying warns against judging a book by its cover. For URLs the opposite is true: **judge the letter by its envelope.** That is what the envelope is for.

A readable URL is an envelope with a full address:

`https://rinie.github.io/2026/06/25/42-towel-at-the-waterline/`

The domain (`rinie.github.io`) is the organisation. The path (`2026/06/25`) is the postmark. The slug (`42-towel-at-the-waterline`) is the subject line. You can evaluate all three before clicking. The trust decision happens before the action. The postman reads the address. You read the address. Both know where it is going.

The opaque redirect is an envelope with no address — just a cryptographic blob that resolves to something. The resolver knows what is inside. You do not. The trust decision is forced to happen after the click, which is too late if the destination is hostile.

This is the same problem as [Do You Trust This Document?](https://rinie.github.io/2026/06/23/wasm-sandbox-vs-policy/) — applied to every link you have ever hovered over.

---

## The Two Layers

The [Gutenberg/Semantic model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) gives the URL its correct anatomy.

The **Gutenberg layer** of a URL is the routing infrastructure — the protocol, the domain resolution, the TCP connection, the HTTP request. The network handles this. You do not need to understand it to use a URL.

The **Semantic layer** is the address on the envelope — the domain that tells you who, the path that tells you what, the slug that tells you the subject. This is the part you evaluate before clicking. This is the part the opaque redirect removes.

The [postman reads the envelope, not the letter](https://rinie.github.io/2026/06/04/postman-reads-envelope/). The router forwards the packet based on the header. Neither opens the payload. The readable URL keeps these two layers cleanly separated — the routing infrastructure is in the protocol and domain, the trust signal is in the path and slug. The opaque redirect collapses them: the routing infrastructure IS the entire URL, and the trust signal is gone.

---

## The VIN at the Barrier

[Marvin remembers the serial number of every car](https://rinie.github.io/2026/06/30/marvin-remembers-serial-number/). The barrier only reads the plate.

The opaque redirect is the VIN handed to the barrier scanner. `AOExmq350wTz2t1IHYmY416px...` is a content-addressed identifier — the Gutenberg identity of the destination, encoded as a long string. It is precise. It is unique. It is completely unreadable as a trust signal.

The plate is the semantic identifier. `42-towel-at-the-waterline` is the plate. You can read it. You can evaluate it. You can decide before you drive through.

The UUID in the URL has the same problem. `/items/3f7a8b2c-de91-4f05-a823-7d6e9c1b0f4e` is a VIN. You cannot evaluate it. You cannot remember it. You cannot share it in speech. The slug is the plate: `/items/my-first-post`. The slug is semantic. The UUID is Gutenberg. [Keep the UUID in the database](https://rinie.github.io/2026/05/27/uuids-are-not-names/). Put the slug in the URL.

---

## Why the Envelope Was Removed

The readable URL passed no tracking information through the link itself. When you clicked `https://rinie.github.io/about/` the server knew you arrived, but not who sent you, which campaign you came from, which device you used, or whether you were the same person who clicked yesterday.

The opaque redirect passes through the platform's analytics system first. `c.gle/AOExmq...` hits Google's servers before it hits the destination. Google logs the click: who, when, from where, on what device, how long before you clicked after receiving the message. The resolver holds all the data. The destination holds none. The resolver is the toll booth at the [waterline](https://rinie.github.io/2026/06/08/hiding-the-waterline/).

Three reasons platforms create opaque URLs:

**Tracking** — the platform measures every click without your knowledge or the destination's cooperation. The envelope was removed to make room for the instrument.

**Monetisation** — the redirect layer can insert ads, paywalls, or consent banners before delivery. The opaque URL is the [enshittification](https://rinie.github.io/2026/06/16/complaint-department-transferred/) infrastructure point.

**Lock-in** — links that pass through the platform's resolver only work while the platform exists. bit.ly links from 2010 that now 404. TinyURL links from 2008. Google+ share links. Every platform that owned a resolver and then closed took every link with it. The resolver died. The dead end street.

The [enshittification arc](https://rinie.github.io/2026/06/16/complaint-department-transferred/) of the URL shortener: step 1, shorten URL (useful). Step 2, add tracking pixel. Step 3, add interstitial. Step 4, require account. Step 5, charge for analytics on your own links. Step 6, shut down, break all links. The complaint department transferred to another dimension. The envelopes all blank.

---

## The QR Code

The QR code is the opaque URL made physical.

You cannot read it. You cannot evaluate the destination before scanning. The Gutenberg identifier (the matrix of black and white squares) reveals nothing about the Semantic destination. You trust the context — this is on a restaurant menu, this is on a business card, this is on a poster in a train station — or you do not scan.

The malicious QR code in a phishing attack exploits exactly this. The envelope was always blank. The trust signal was always absent. The [ambiguity was not a bug](https://rinie.github.io/2026/06/22/ambiguity-is-not-a-bug/) — it was the feature that made the attack possible. Hiding the destination is not convenience. It is the hidden seam.

---

## The Number Is Still Yours

The readable URL on your own domain survives as long as you run the server. The resolver is yours. The plate is yours. [Your number is still yours](https://rinie.github.io/2026/06/15/gutenberg-your-next-phone/).

`https://rinie.github.io/2026/07/10/should-i-click/` will resolve as long as GitHub Pages runs and rinie.github.io is maintained. No third-party resolver. No platform dependency. The envelope has a permanent address. The postman knows the route. The link does not die when a startup shuts down.

The opaque redirect is renting the resolver. Someone else holds the plate. When they leave, the link goes with them.

**This is also why the base URL, not each individual link, is the thing worth getting right.** Every link on this site is written relative to one configured base — `rinie.github.io` sits in exactly one place, the site configuration, not copy-pasted into every post's every link by hand. Move the whole site to a different domain and every internal link, every "part of the series" footer, every cross-reference between posts updates automatically on the next deploy, because none of them were ever hardcoded destinations — they were always semantic paths, resolved against whatever the base URL happens to be at build time. The individual posts never needed to know the domain. They only ever needed to know the path. That's the same envelope-and-plate separation this whole post has been describing, just applied to the site's own internal plumbing instead of the links pointing outward from it — the base URL is the one Gutenberg fact, set in one place, and every semantic path in every post rides on top of it without having to be told.

Own your domain. Use readable URLs. Put the semantic information in the path where the reader can see it before clicking. Keep the UUID in the database.

The envelope is the trust infrastructure. It was always there. The platform removed it on purpose.

Your hesitation before clicking the opaque link is the system working correctly. The user who does not click is not the problem. The unreadable link is.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [The Postman Reads the Envelope, Not the Letter](https://rinie.github.io/2026/06/04/postman-reads-envelope/) on the two-layer structure that makes trust possible, [UUIDs Are Not Names](https://rinie.github.io/2026/05/27/uuids-are-not-names/) on Gutenberg identifiers in the semantic layer, [Ambiguity Is Not a Bug](https://rinie.github.io/2026/06/22/ambiguity-is-not-a-bug/) on the resolver that cried wolf, [Hiding the Waterline Makes You Drown Without Knowing Why](https://rinie.github.io/2026/06/08/hiding-the-waterline/) on the hidden seam, [The Complaint Department Has Been Transferred to Another Dimension](https://rinie.github.io/2026/06/16/complaint-department-transferred/) on enshittification, [Gutenberg: Your Next Phone Will Be a Different Make on a Different Carrier. Your Number Is Still Yours.](https://rinie.github.io/2026/06/15/gutenberg-your-next-phone/) on owning the resolver, and [Do You Trust This Document? No, I Have to Read It First.](https://rinie.github.io/2026/06/23/wasm-sandbox-vs-policy/) on trust decisions that arrive too late.*
