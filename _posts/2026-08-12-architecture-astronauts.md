---
layout: post
title: "Architecture Astronauts: Solving the Template of a Problem Instead of the Problem"
date: 2026-08-12
tags: [gutenberg-semantic, joel-spolsky, architecture-astronauts, log-cabin, walled-garden, hailstorm, groove, live-mesh]
level: general
description: "Joel Spolsky coined the term in 2001 watching Microsoft's Hailstorm. Seven years later he watched the same team pitch the same architecture again, called Live Mesh. The hallmark of an architecture astronaut isn't ambition — it's solving something that looks like the template of a lot of problems, instead of solving an actual one."
---

Joel Spolsky coined a term in 2001 that this series has been circling without quite naming: the architecture astronaut. *"The hallmark of an architecture astronaut is that they don't solve an actual problem — they solve something that appears to be the template of a lot of problems."* He wrote it watching Microsoft's Hailstorm, a comprehensive identity-and-sync platform that promised your data would live in the cloud, accessible everywhere, under Microsoft's control. It launched with enormous fanfare and quietly died because [nobody needed a place for all their stuff, and nobody trusted Microsoft with it](http://www.joelonsoftware.com/items/2008/05/01.html).

Seven years later, in 2008, he watched the same company's Chief Software Architect — Ray Ozzie, who'd previously built Lotus Notes and then Groove, a peer-to-peer platform Spolsky had already filed under the same critique — announce Windows Live Mesh. Same promise. All your devices, working together, anywhere access to your information. *"Isn't that exactly what Hailstorm was supposed to be? I smell an architecture astronaut."*

---

## The Template, Not the Problem

What makes an architecture astronaut isn't technical incompetence. Spolsky is careful about this — he calls Groove "a nice architecture," genuinely well engineered, solving the server-bottleneck problem that crippled Notes at scale. The failure is a level up: the astronaut builds a general-purpose platform for a category of problem — synchronisation, in this case, repeatedly, across three different products from overlapping teams — instead of a specific solution to a problem an actual user has right now.

*"Synchronizing files is just not a killer application. I'm sorry. It seems like it should be. But it's not."* Spolsky's point isn't that sync is unimportant. It's that sync-as-architecture is a template dressed up as a product. The killer app, if there ever is one, turns out to be something narrow and specific built on top of good plumbing — never the plumbing itself, marketed as the destination.

This is [the log cabin from a different angle](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/). The log cabin builds a comprehensive world instead of resolving against what already exists. The architecture astronaut builds a comprehensive *solution* instead of resolving against an actual, specific need. Both mistake completeness for value. Ada's committee tried to specify a language comprehensive enough for every domain the US Department of Defense could imagine needing. Hailstorm tried to specify a platform comprehensive enough for every kind of data synchronisation anyone might ever want. Neither failure was a failure of engineering skill. Both were a failure to ask what problem, specifically, a specific person needed solved today.

---

## The Tell: The Same Pitch, Different Decade

What makes Spolsky's second essay sharper than the first is the recurrence. Hailstorm dies in 2001. Groove — built by the person who'd go on to lead Microsoft's architecture — gets the same critique in the same year. Groove gets acquired by Microsoft in 2005. Its designer becomes Chief Software Architect. Three years later, the same organisation, with the same person now in charge of platform strategy, ships Live Mesh: *"Imagine all your devices — PCs, and soon Macs and mobile phones — working together to give you anywhere access to the information you care about."* Word for word, the Hailstorm pitch, seven years and one acquisition later.

That recurrence is the actual diagnostic worth borrowing. A single ambitious, comprehensive platform that fails might just be bad timing or bad execution. The same comprehensive platform, pitched again by the same people once they've acquired more power to build it, and failing again for the same reason, is evidence the failure was never about execution at all. Nobody along the way stopped to ask what specific, narrow thing a specific person actually needed — because architecture astronauts, by Spolsky's own account, are seduced by the elegance of the general case and treat the absence of a killer app as a marketing problem rather than a signal.

---

## The WAP Coupon and the Problem Nobody Has

Spolsky's sharpest example is the smallest one, buried in the Groove exchange: *"the idiot WAP people who talk endlessly about how you'll walk by a Starbucks and the GPS in your phone will coordinate to beam you a coupon for that Starbucks."* He calls this out precisely: *"it solves the one problem that coffee shops DON'T have, namely, advertising to people who are standing right in front of the store."*

That's architecture-astronaut thinking distilled to one sentence: an elaborate technical pipeline — GPS, location beacons, real-time coupon delivery — built to solve a problem that never existed, because the coffee shop's sign was already doing that job for free. [The barista already knows your usual](https://rinie.github.io/2026/07/31/smart-coffee-machine-usual-cuppa/) without any of this infrastructure; the WAP coupon system solves the template of "getting information to a nearby customer" while the actual problem — a customer walking past who already sees the sign — was never a problem in the first place.

---

## Ship the Applet, Not the Architecture

Spolsky's advice to Groove's designer is the practical antidote, and it's worth keeping as plainly as he wrote it: *"you shipped a product, and sold it to people, it's great, I believe you. But if you want it to be The Next Great Thing it has to be more than architecture, it has to enable things that people really need."* The architecture doesn't sell the platform. The specific, narrow thing built on top of it does — the killer app, not the template that made the killer app theoretically possible.

This is the same discipline [Use-specialisation argues for at the code level](https://rinie.github.io/2026/08/08/use-specialisation/): stay wide underneath, but let the point of actual use — the specific problem, the specific person, the specific applet — be where the real value gets built. An architecture astronaut inverts that. They perfect the wide, general layer and never quite get around to the narrow, specific thing anyone would actually pay for. The platform becomes the destination instead of the plumbing, and plumbing, however elegant, was never the thing anyone was trying to buy.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Wirth's Law Only Holds If You Recompile the Whole Iceberg](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/) on the log cabin and the walled garden, and [Use-Specialisation](https://rinie.github.io/2026/08/08/use-specialisation/) on staying wide until the point of actual, specific use.*

Sources: [Are the Groove Designers Architecture Astronauts?](http://www.joelonsoftware.com/articles/fog0000000011.html) and [Architecture astronauts take over](http://www.joelonsoftware.com/items/2008/05/01.html), Joel Spolsky, Joel on Software, 2001 and 2008.
