---
layout: post
title: "Google Says Welcome Home. Your Car Just Sold You a Home Button."
date: 2026-08-01
tags: [gutenberg-semantic, ux, def-push, use-pull, observability, ota, feedback, google, audi, bsh, samsung, streetview, privacy]
level: general
description: "Google Maps learned where you live by watching you. Your car's nav has a form field labeled Home. The difference is whether the company thinks the product is finished at the point of sale. BSH's oven app is worse than Samsung's not because it's clumsier, but because BSH was never structurally built to close the loop. And there's a real line between welcome home and I know where you live — Street View draws it at the front door and stops."
---

Google Maps says "Welcome home." Your Audi's built-in navigation has a field labeled Home that you type an address into, once, during setup, and it never asks again or notices if you moved.

Both systems have the same raw material — GPS traces, repeated trips, timestamps. Audi's version arguably has better data, since it's the same car, every day, for years. The difference isn't sensors. It's what each company thinks happens after the sale.

---

## Finished at the Point of Sale

A car, a coffee machine, a washing machine ships as a finished object. The BOM — bill of materials, every part, every capability, every setting — is complete the day it leaves the factory. The manufacturer's job was to build a correct, complete machine. That job is done. What happens in your kitchen for the next ten years is not, in this model, the manufacturer's concern.

Google's product was never finished at the point of anything. Search results, ad targeting, Maps routing — all of it exists to get better every single day, forever, because the business model requires it. A search engine that stopped learning in 2010 would be worthless today. The company's entire structure is built around continuous observation and continuous adjustment, because standing still is death.

This is the same organizational split from [Borg's Arrogance](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/): the capability that gets built is the one whose absence costs the org something. Google dies without behavioral inference. Audi ships a perfectly functional car without it. Nobody at Audi feels the pain of the missing "Welcome home" — you do, quietly, tapping through the same setup screen on a car you've owned for three years.

---

## Why OTA and Apps Land Badly

This explains something odd: car companies and appliance makers keep adding over-the-air updates and companion apps, and the results are so often disappointing. The capability for continuous improvement exists. It rarely produces continuous improvement.

An OTA update from a company that thinks the product was finished at sale mostly ships bug fixes and, occasionally, a new BOM feature — another setting, another mode, another item for the menu. It is a delayed continuation of the same manufacturing mindset: here is a new part, add it to the list. What it almost never does is watch what you actually did with the car for the last six months and change the defaults accordingly. The update mechanism is present. The organizational habit of using it to *learn* is not.

Google's updates work differently because the whole company already runs on the assumption that yesterday's behavior should change tomorrow's product. The update pipeline exists to serve continuous inference, not to patch a static object. Same delivery mechanism — OTA push, app update — completely different purpose behind it.

---

## Observability Cuts Both Ways

Once a manufacturer builds the sensors and the pipeline needed to observe use patterns, that same capability faces two directions and there's no guarantee which one gets built.

Pointed inward, at the product: your coffee machine learns your usual, your car learns your commute, your washing machine learns your typical load and stops asking. This is Use-Pull done right — the same [confirmation loop](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/) pattern from good resolver design, just applied to appliances instead of DNS. Watch, infer, confirm once, stop asking.

Pointed outward, at the business model: the same behavioral data becomes something to sell. Your driving patterns become insurance risk scoring. Your coffee habits become a subscription upsell trigger. Your appliance usage becomes a dataset licensed to a third party. The observability infrastructure is identical in both cases — sensors, logging, a pipeline that turns raw signal into a model of what you do. Only the destination differs: does the inference come back to you as a better product, or does it leave the device as a product about you?

This is why "smart" appliances get a bad reputation even from people who'd genuinely benefit from a machine that learns their usual. The infrastructure that could say "same as yesterday?" is frequently the same infrastructure logging every use to a server whose primary customer isn't you. Users have learned, correctly, to be suspicious of "connected" as a feature, because connected has too often meant *harvested* rather than *improved*.

---

## Clumsy Is Not the Same as Worse

Not every disappointing connected appliance fails for the same reason, and it's worth separating two different complaints that get lumped together as "bad app."

A BSH oven (Bosch, Siemens, Neff, Gaggenau all sit under one roof) and a Samsung oven can both have clumsy apps — confusing menus, awkward setup flows, icons that don't mean what they look like they mean. That's a craft problem. Bad UX people, or good UX people without enough budget, produced something unpolished. It's fixable by hiring better designers.

But BSH's app tends to be *worse* in a different, structural sense, and it's not a craft problem. BSH's core competency, budget, and century of institutional pride live entirely in the metal — the burner, the seal, the door hinge engineered to survive twenty years of daily use. Home Connect is a division bolted on to satisfy a feature checklist, not a company reorganized around continuous behavioral inference. Nothing in BSH's business model rewards making the app quietly learn your Tuesday roast schedule over time, so nobody builds toward it, however good the individual UX designers on that team might be.

Samsung's oven app can be equally clumsy on a bad day, but Samsung is not fundamentally an oven company with a software afterthought — it's a conglomerate whose phone and TV divisions live or die by shipping software that has to keep improving, because that's the entire premise of a phone business. The oven team inherits a company-wide habit of treating connected software as something that should get better, even if this particular app hasn't yet. Clumsy is a bug. Structurally incapable of ever closing the loop is a different kind of problem, and it's the one that predicts whether the app improves in five years or stays exactly as annoying.

---

## Welcome Home, Not "I Know Where You Live"

There's a real reason people flinch at "smart" appliances even when the underlying idea — a machine that learns your usual — is exactly what they want. The same inference that produces a warm "Welcome home" can just as easily produce the cold, threatening version of the same fact: a stranger, or a company, demonstrating they know where you live as a display of power rather than a convenience.

Google Street View drew this line early and it's worth naming as the calibration example. Street View shows your front door — the public-facing side of your house, visible to anyone walking past anyway — and stops there. It blurs faces and license plates. It does not, and structurally cannot, peer through your windows. The boundary isn't incidental; it's the entire difference between a useful public map and a surveillance product. The inference stops exactly where "helpful" turns into "watching you."

The barista who remembers your usual order is the same calibration, done by a person instead of a policy. They know what you order. They do not know, and have no way to find out, what you do at home, who you live with, or what's in your fridge. The relationship is scoped to the counter. That scoping is what makes "the barista knows me" feel warm rather than watched.

A coffee machine that says "same as yesterday?" is doing the barista's job — scoped to the counter, so to speak. A coffee machine (or a car, or a doorbell camera) that reports your usage patterns to a server whose real customer is an advertiser or a data broker has crossed from Street View's front door into the window. The sensor and the inference can look identical from the outside. The only way to tell which one you're holding is to ask where the model goes once it's built — back to you as a shortcut, or out to someone else as a product. That's the actual dividing line between "welcome home" and "I know where you live."



The dividing line between "welcome home" and "I know where you live" is also the same test that separates BSH from Samsung, and Audi from Google Maps: does the inference serve you, or does it serve whoever owns the pipeline.

## The Test

The test for whether built-in observability is serving the Use side or extracting from it is simple: does the inference shorten your next interaction, or does it just get reported somewhere?

A car that learns your commute and pre-warms the seat, adjusts the mirrors, and queues your usual playlist before you've touched anything is observability serving you. A car that learns your commute and sells the pattern to a data broker is observability serving someone else, using your car as the collection point. The sensor is the same. The pipeline might even be the same. The difference is entirely in what happens to the model once it's built — and whether the company that built the pipeline was ever organizationally capable of imagining the first use case in the first place.

Companies that think the product is finished at sale rarely build the first kind, because building it requires believing the relationship with the customer continues past the transaction. They're more likely to build the second kind, because selling data fits a finished-object mindset perfectly — the car is done, but the exhaust it produces (your usage data) is a new product they can keep selling long after the original sale closed.

Google says welcome home because Google's business requires learning from you forever. Your car sold you a form field because your car company's business ended at the parking lot, and the OTA update mostly just adds new items to a bill of materials that was already finished the day you drove it off the lot.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Why Does My "Smart" Coffee Machine Not Know My Usual Cuppa?](https://rinie.github.io/2026/07/31/smart-coffee-machine-usual-cuppa/) on the barista who watches versus the app that asks, [Borg's Arrogance](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) on capabilities that harden only when their absence costs the org something, and [The Complaint Department Has Been Transferred to Another Dimension](https://rinie.github.io/2026/06/16/complaint-department-transferred/) on what happens when the resolver becomes an extractable resource.*
