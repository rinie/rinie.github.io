---
layout: post
title: "\"Obsolescence Was Not an Issue\": When the Cloud Dies on Purpose"
date: 2026-08-10
tags: [gutenberg-semantic, connected-cars, def-push, resolver, onstar, acura, obd, carplay, hardcoded]
level: general
description: "Acura's 4G hardware still worked when they shut off AcuraLink anyway — their own words: obsolescence was not an issue. That single sentence separates two failures the industry usually blurs together: a real Gutenberg constraint (3G sunset) and a pure Def-Push choice dressed up as inevitability. The OBD port sat there the whole time, a real standard nobody built a safety-feature contract on top of."
---

An Ars Technica piece on connected-car obsolescence buries the sharpest sentence in the whole industry's history of killing cloud services in a single parenthetical about Acura: *"Even though the vehicles and their modems could connect, Acura chose to stop supporting the old AcuraLink backend stack and feature set because the company determined it wasn't worth it. Obsolescence was not an issue."*

That sentence is worth sitting with, because it quietly separates two failures the rest of the article — and most coverage of this topic — blurs into one story called "old tech becomes obsolete." They are not the same failure, and the vocabulary this series has been building all year separates them cleanly.

---

## Two Different Deaths

3G's shutdown was a real Gutenberg-layer event. AT&T, T-Mobile, and Verizon repurposed spectrum for 4G and 5G in 2022, and every device still speaking only 3G — Lexus Enform, plenty of Porsche, Nissan, and VW systems — lost its carrier the way a landline loses dial tone when the exchange gets decommissioned. Nobody chose to hurt those owners specifically. The physical layer they depended on stopped existing.

Acura's shutdown was not that. The 4G hardware worked. The modems could connect. The company killed the backend anyway, on a pure cost-benefit calculation about whether maintaining that stack was worth it. BMW did something close to the same thing with 3G ConnectedDrive — announced a hard stop date and terminated service for owners whose connectivity, by the article's own account, "functioned perfectly."

The industry's messaging treats both as the same inevitability — technology moves on, nothing lasts forever. Acura's own stated reason proves that framing false at least once, on the record. When "obsolescence was not an issue" is the company's own explanation, the shutdown wasn't physics. It was [a Def deciding the maintenance cost no longer justified keeping a promise it had made to buyers years earlier](https://rinie.github.io/2026/08/01/finished-at-the-point-of-sale/) — the finished-at-sale mindset, but sharper: not merely failing to keep improving the product, actively switching off something that already worked.

---

## The TCU Was a Hardcoded Fact, Not a Resolved Name

The deeper mechanism under the 3G story is worth naming precisely, because it's the same failure this series has traced through DNS, npm, and Java package names. A telematics control unit built with a 3G-only modem hardcoded "the current cellular generation" into a piece of physical hardware with no upgrade path — [the same mistake as a package name with no resolver behind it](https://rinie.github.io/2026/07/29/java-reversed-hierarchy-forgot-resolver/), just cast in silicon instead of a config file. The car didn't ask "what network is currently available" at connect time. It assumed one, permanently, at the factory.

The manufacturers who handled the 3G sunset best in the article's account are the ones who had, whether by luck or design, left themselves a resolver: Genesis's SVLTE-type telematics could be upgraded in software because the hardware wasn't purely 3G-locked. Honda pushed free OTA upgrades to 4G LTE ahead of the cutoff. GM preserved core OnStar service through a software update on the same aging back-end stack. Where the hardware had any abstraction between "connectivity" and "specific radio generation," the transition was survivable. Where it didn't — pure 3G-only TCUs, no software escape hatch — the service died overnight, and owners like BMW's i3 drivers paid up to $1,400 out of pocket for an unofficial fix the manufacturer wouldn't provide.

None of this had to be improvised per-manufacturer, case by case, with wildly inconsistent remedies from "free refund" to "$1,400 DIY retrofit." A [SIM or eSIM abstraction](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) that resolved to whatever network generation was actually current, decoupled from a specific hardcoded radio in the TCU, would have made the 2022 3G sunset a non-event for every car built with it — the same way your phone doesn't care which generation the tower nearest you happens to be running today.

---

## The OBD Port Was Already There

The article notes, almost in passing, that aftermarket telematics devices exist and can plug into the OBD diagnostic port — but they "generally can't duplicate OEM functions such as remote lock/engine start, automatic crash notification, or SOS features because they sit on the OBD port rather than on the carmaker's proprietary telematics stack."

That sentence describes a walled garden with a working door nobody bothered to install a lock in. OBD-II is a real, mandated, decades-old physical standard — [exactly the kind of junction box this series keeps pointing at](https://rinie.github.io/2026/08/07/taste-comes-last/), present in essentially every car sold in the US and EU since the late 1990s specifically so third parties could build against a stable interface without needing the manufacturer's cooperation. The port was never the limitation. The limitation was that manufacturers never exposed remote lock, crash notification, or SOS as semantic capabilities reachable through that already-standard physical connector. They kept those functions locked inside the proprietary stack that then had an expiration date nobody told the buyer about.

An OBD-reachable emergency-call function would have survived every 3G sunset, every Acura cost-cutting decision, every BMW contract expiration, because it would never have depended on any single manufacturer's backend staying funded. The standard existed. The Def chose not to build the bridge to it for the one category of feature — safety — where that bridge would have mattered most.

---

## CarPlay Is Where the Owners Ended Up Anyway

The article's own conclusion is the same one [this series reached from the other direction](https://rinie.github.io/2026/08/05/purchase-frequency-gates-learning/): owners who lose cloud connectivity increasingly fall back on Apple CarPlay and Android Auto, "phone-driven connectivity" that's "proven both dependable and virtually seamless for some drivers." The phone brings its own always-current network generation, its own crash detection, its own SOS. The car's own infotainment CPU ages on a fifteen-year purchase cycle it can never win against a two-year phone refresh; the manufacturer's proprietary backend depends on a business decision that can end on a date the owner never agreed to. The phone, resolved fresh at every connection, outlives both.

The article's proposed fix — "modular software-defined architecture," letting owners physically swap hardware and upgrade software as technology advances — is the industry rediscovering, fifteen years late and at real cost to millions of owners, the same principle [Android built into itself from ART down to Mainline](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/): decouple the part that ages fast from the part you're not going to replace, build the resolver at the boundary between them, and stop hardcoding a Gutenberg fact — a radio generation, a backend contract's funding — into hardware that's expected to outlive it.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Google Says Welcome Home. Your Car Just Sold You a Home Button.](https://rinie.github.io/2026/08/01/finished-at-the-point-of-sale/) on the finished-at-sale mindset, [Your Oven Gets One Chance Every Fifteen Years](https://rinie.github.io/2026/08/05/purchase-frequency-gates-learning/) on why CarPlay wins, and [The Meterkast Pattern](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) on the demarc that should have made the 3G sunset a non-event.*

Source: [When your vehicle outlives its cloud: What happens next?](https://arstechnica.com/cars/2026/07/when-your-vehicle-outlives-its-cloud-what-happens-next/), Michael Harley, Ars Technica, July 21, 2026.
