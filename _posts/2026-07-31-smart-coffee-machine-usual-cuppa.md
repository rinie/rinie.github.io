---
layout: post
title: "Why Does My \"Smart\" Coffee Machine Not Know My Usual Cuppa?"
date: 2026-07-31
tags: [gutenberg-semantic, ux, def-push, use-pull, appliances, coffee, gemba, inmates]
level: general
description: "A good barista knows your usual after three visits. My connected coffee machine has two years of order history, a camera pointed at the drip tray, and a full sensor suite — and still asks me to pick from a menu at 07:00. The cup is already on the tray. The pattern is already in the log. The usual is already known. It just isn't being used."
---

A good barista knows your usual. Not because they memorised a file — because they paid attention. You came in at 08:15 on Tuesday, you ordered a flat white, you sat by the window. Wednesday same time, same order. By Thursday they start making it when they see you come through the door. The Use signal (you, here, now, same as always) is observed and acted on. You didn't fill in a form.

A connected coffee machine has more data than any barista. It has your full order history timestamped to the minute. It has weight sensors, flow sensors, a temperature probe, and on several models a camera pointed directly at the drip tray. It knows what you made last Tuesday at 07:15. It knows the cup you used. It knows how long you stood there waiting.

And every morning it asks you to start from scratch.

---

## The Cup Is Already There

Before you touch the app, you have already communicated your intent. You chose a cup. You put it on the tray. An espresso cup is not a lungo glass. A travel mug is not a flat white cup. The physical object is the Use signal — present, unambiguous, observable — and the machine is looking directly at it.

The app that ignores the cup and opens a category menu (Espresso / Lungo / Cappuccino / Speciality) has decided its decision tree matters more than the object on the tray. The UI is performing the machine's capabilities rather than serving the person who put the cup down. This is [the inmates problem](https://rinie.github.io/2026/06/20/nothing-is-confusing-to-me/) at 07:00: the engineer who designed the menu knows why each parameter matters, so exposing all of them feels thorough. The person who wants coffee doesn't care about any of it. The 90% signal — same cup, same time, same day of the week — is being ignored in favour of a UI that demonstrates the machine knows many things.

The machine does know many things. It is choosing not to use them.

---

## What the Barista Does That the App Does Not

A barista's knowledge of your usual is not stored in a database. It is built from observation: what you ordered, when you come in, how you take it, what you didn't finish. The resolver is attention, not a form you filled in once during onboarding.

The connected app has better data than any barista and uses less of it. BSH Home Connect, Nespresso's app, Philips' LatteGo — all have full order histories, all have timestamped logs, all know which programmes ran in what order on which mornings. None of them default to "same as usual?" None of them surface last Tuesday's order as the starting point. All of them open a blank menu, every time, as if you just bought the machine.

This is not a data problem. The data is there. It is a Use-Pull problem: the app was designed from the Def side outward, presenting the machine's full capability space rather than watching what the Use side actually does and reflecting that back. A Use-Pull app watches the pattern, names it, and offers it. A Def-Push app presents options.

The barista equivalent of the connected app would be: every morning, as you walk in, hand you a laminated card listing every drink on the menu and ask you to point at one. Thorough. Useless.

---

## Last Known Good, At 07:00

The [DNS post](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/) made the same argument at the infrastructure layer: the resolver that remembers last known good and offers it as the default is more useful than the resolver that starts fresh every time. The confirmation loop is the mechanism — if last Tuesday's settings produced a good result, they are the right starting point for this Tuesday. If something changed (different cup, different time, someone else using the machine), the departure from the pattern is the signal worth surfacing, not the pattern itself.

A coffee app that said "flat white, same as Tuesday — start?" would serve the 07:00 use case correctly. The confirmation is one tap. The departure ("actually a lungo today") is one tap from the same screen. The full menu is still available, one more tap away, for the days when something genuinely different is wanted.

Instead the app presents the full menu first, every time, because the Def side cannot imagine that the Use side might not want to exercise the full decision space on a Tuesday morning before the first cup is made.

---

## The Moka Pot Has No UI

A Moka pot on the stove has no app, no history, no sensor. It has a fill line. You fill it to the line. The line is the resolver — it remembers nothing and needs to remember nothing, because the physical object encodes the decision. The amount of coffee is determined by the pot's size, which you chose when you bought it, which reflects how much coffee you make. The Use signal is baked into the hardware at purchase time and never asked about again.

This is not a failure of the Moka pot. It is the correct design for a tool that does one thing. The connected machine's promise was that connectivity would improve on this — more flexibility, more personalisation, better outcomes over time. The promise was Use-Pull: the machine learns what you need and gets better at serving it. The delivery was Def-Push: the machine learned what it could do and made sure you knew all of it.

A €500 connected machine with a camera, a sensor suite, and two years of your order history should be able to do what a barista does after three visits. Watch. Remember. Offer. The cup is already on the tray. The pattern is already in the log. The usual is already known.

It just isn't being used.

---

## YAGNI Ignorance Sux

There is a principle in software called YAGNI — You Aren't Gonna Need It. Don't build features nobody asked for. The connected coffee machine ignored it in the wrong direction: it built every feature the Def side could imagine (seventeen brew profiles, cloud sync, firmware updates, a social sharing button) and skipped the one feature the Use side actually needed. Not because it was hard. Because nobody on the Def side stood in a kitchen at 07:00, half-awake, cup on the tray, and asked why the app was showing them a menu.

YAGNI applied correctly cuts the features nobody uses. But you have to [go to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) to know which features those are. YAGNI without the Gemba walk is just the Def side deciding what the Use side probably needs — which is how you end up with seventeen brew profiles and no memory of last Tuesday. The barista who forgets your usual after two years isn't smart. They just sux.

---

 the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Nothing Is Confusing to Me](https://rinie.github.io/2026/06/20/nothing-is-confusing-to-me/) on the inmates who can't see the confusion they create, [Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on watching the Use signal where it actually lives, and [It Is Always DNS](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/) on last known good as the right default.*
