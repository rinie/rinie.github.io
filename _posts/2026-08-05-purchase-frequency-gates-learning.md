---
layout: post
title: "Your Oven Gets One Chance Every Fifteen Years"
date: 2026-08-05
tags: [gutenberg-semantic, ux, feedback, pace-layering, purchase-frequency, ota, learning, tesla, loyalty, carplay, consumable]
level: general
description: "A phone company gets fresh purchase feedback every two years. An oven company gets one every fifteen. Some touchpoints never reach the manufacturer at all — you fuel up weekly and change tyres every few years, both at third parties. And the car's infotainment CPU ages like a phone but sits inside a car's purchase cycle with no consumable-replacement channel — which is exactly why CarPlay won. It borrows a refresh cycle the car industry never built."
---

Your phone gets meaningfully better every year or two. Your oven has been roughly the same clumsy machine since the model was designed, and the replacement you buy in fifteen years will likely be clumsy in the same ways. This isn't a difference in how much either company cares about you. It's a difference in how many times the feedback loop between "sold a product" and "learned something from selling it" has had the chance to close.

---

## Purchase Frequency Is a Pace Layer Nobody Named

[Brand's shearing layers](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) order a building by how fast each part changes. Consumer products have the same structure, but the relevant clock isn't usage frequency — how often you touch the thing — it's purchase frequency: how often the manufacturer gets a new round of buyers making a fresh decision, generating fresh data, fresh reviews, fresh returns, fresh support tickets.

- **Phone**: replaced every 2–3 years
- **Laptop**: every 3–5 years
- **TV**: every 7–10 years
- **Car**: every 5–8 years, shrinking with leasing
- **Built-in oven**: every 15–20 years
- **Boiler, roof, foundation**: decades, sometimes once per owner

That's Brand's pace ordering, recalculated on a completely different axis — not how fast the object itself decays, but how fast the manufacturer's feedback loop can complete a full cycle: design, sell, observe what happened, redesign.

---

## The Loop Needs to Close to Teach Anyone Anything

A company that sells you a phone every two years gets a fresh cohort's worth of purchase decisions, support calls, one-star reviews, and churn data roughly ten times per generation of an oven manufacturer's product. Ten iterations of "here's what confused people, here's what they returned, here's what they asked support about" versus one. Not because phone engineers are smarter or care more about the user than oven engineers. Because the phone company has had ten chances to learn something and the oven company has had one.

This is the mechanism underneath [the finished-at-sale diagnosis from a few days ago](https://rinie.github.io/2026/08/01/finished-at-the-point-of-sale/). It's not only that some companies think the product is done at the point of sale and others don't — even a company with every intention of learning from its customers is structurally limited by how often it gets a new batch of customer decisions to learn from. Google Maps improves constantly in part because Google's product mindset is continuous, but also because "how do people search for places" generates billions of fresh data points daily. An oven company waiting fifteen years between purchase cycles is trying to learn a lesson from a classroom that graduates and leaves before the teacher has finished grading the first assignment.

**By the time the lesson would land, everything that could receive it has moved on.** The team that designed the confusing menu has been reorganised, promoted, or left. The product line has been renamed twice. The company that made your oven fifteen years ago and the company that makes the one you'd buy today share a badge and maybe a factory, but the specific people who could have learned "nobody understood the delay-start feature" are, statistically, gone.

---

## Two Different Loops, Two Different Clocks

There are actually two separate learning loops here, running on two separate clocks, and conflating them is easy to do and worth avoiding.

**Within-product learning** runs on usage frequency — [the barista/coffee-machine loop](https://rinie.github.io/2026/07/31/smart-coffee-machine-usual-cuppa/): does *this specific device* learn your usual over the years you own it. That clock is fast for almost everything — you use your oven weekly, your car daily, your phone constantly — so there's no structural excuse for it being absent. A coffee machine that fails to learn your usual after two years of daily use isn't limited by purchase frequency. It had thousands of observations available and used none of them.

**Across-generation learning** runs on purchase frequency — does the *company* get better at designing the *next* oven based on what it learned from this one. That's the slow clock, and it's the one that actually explains the phone/oven gap. A phone company gets both loops running fast: frequent usage feeds within-product learning (if they build it), and frequent purchases feed across-generation learning regardless. An oven company only ever gets the first loop for free — you use it daily, so the data exists — but has almost no structural mechanism forcing it to close the second loop, because the next purchase decision that could carry the lesson forward is fifteen years away.

---

## Sux Tolerance Is a Function of the Clock

This explains something that otherwise looks like inconsistency: users tolerate a clumsy oven far longer than they'd tolerate an equally clumsy phone, and companies respond to exactly that tolerance rather than fighting it. If nobody switches ovens over a bad menu — because switching means waiting another fifteen years anyway, and the alternative brands are roughly as clumsy — there is no market pressure forcing the loop to close faster. The slow purchase clock doesn't just limit how much a manufacturer *can* learn. It limits how much the market makes them *need* to.

Contrast a car dashboard, which now updates via OTA on phone-like timescales even though the car itself is bought on a 5–8 year clock. Car companies broke the old pace-layering assumption deliberately — Tesla and its imitators decoupled the *software's* learning clock from the *hardware's* purchase clock, so the fast loop (usage, OTA feedback, weekly updates) runs independently of the slow one (buying a new car). That's the same move [cloud made to Brand's Site](https://rinie.github.io/2026/08/04/cloud-breaks-the-pace-layers/): decouple the thing that needs to stay fixed from the thing that's expensive to replace, and let the fast layer iterate on its own schedule.

Nothing structurally prevents BSH from doing the same thing to Home Connect that Tesla did to its dashboard — the oven's mechanical core stays on its fifteen-year clock, but the software layer, if genuinely decoupled and genuinely OTA-updated with real behavioral learning behind it, could run on a much faster loop, gathering across-generation lessons at usage-frequency speed instead of purchase-frequency speed. The technology to do this already exists in the same house, in the coffee machine's competitor category. The question is whether the organisation is structured to bother, which circles back to [whether the company was built around continuous learning or finished-at-sale](https://rinie.github.io/2026/08/01/finished-at-the-point-of-sale/) in the first place. Purchase frequency sets the ceiling on how fast learning happens by default. It doesn't have to be the ceiling on how fast learning happens at all.

## Not Every Touchpoint Feeds Back to the Company That Could Learn From It

There's a complication inside a single car that sharpens the whole argument: not every frequent interaction with the product actually reaches the manufacturer at all.

You fill up with fuel weekly. You do not change your own tires, and when you do get them changed, it's almost always at a third-party garage, not the car manufacturer. Both are genuinely frequent touchpoints in the ownership experience — arguably more frequent than anything the manufacturer directly observes between the sale and the next one. But fuel is a transaction with a petrol station, and tyres are a transaction with a tyre shop. High frequency, zero feedback to the company whose next design could actually use it. The loop isn't just slow for the car manufacturer — for these touchpoints, it's not connected at all. The data exists. It's just flowing to a competitor-equivalent third party who has no stake in the manufacturer's next model.

This is why Tesla's vertical integration into charging infrastructure and direct-owned service centres is a feedback-loop strategy as much as it's a business strategy. Recapturing the charging touchpoint — something you interact with far more often than you'll ever visit a dealership — pulls a high-frequency signal back inside the loop that a traditional manufacturer leaked to third-party fuel stations for a century. The OTA dashboard update from earlier in this post and the owned charging network are the same move twice: find the touchpoint that happens often, and make sure the lesson it could teach actually reaches the people who build the next car, instead of evaporating into a transaction with someone else entirely.

**Which touchpoints deserve the investment isn't just about frequency — it's frequency weighted by influence on the next purchase.** A company with limited attention shouldn't try to instrument everything equally. The fuel-up is frequent but close to irrelevant to brand loyalty — nobody chooses their next car based on how the fuel cap works, because fuel caps are commodity and undifferentiated across the entire market. Tyre quality, by contrast, is infrequent but genuinely shapes the next purchase decision — a driver who had two blowouts remembers that at trade-in time, whether or not the manufacturer ever heard about it. The right question for any touchpoint isn't just "how often does this happen" but "how much does getting this right or wrong move the odds this customer buys from us again" — and the touchpoints worth fighting to recapture into the loop are the ones that are both frequent enough to generate real data and consequential enough to actually change the next sale.

## Why the Manufacturer Lost the Infotainment Battle Before It Started

The car's infotainment CPU and screen age like a consumable — silicon and software both go stale on something closer to a 2–3 year cycle, the same clock as a phone. But it's bolted into a product bought on a 5–8 year cycle, and unlike tyres, there's no established channel for replacing it mid-ownership. Nobody takes their car to a shop every three years for an infotainment refresh the way they do for tyres. The manufacturer never built that consumable-replacement business, so the part that ages fastest in the whole vehicle is the one part with no scheduled renewal at all.

This is the mismatch that made CarPlay and Android Auto inevitable rather than merely convenient. [The white-line post noted the seam holding](https://rinie.github.io/2026/07/27/dont-hide-the-fence/) when a five-year-old car's infotainment gets more capable purely because the driver bought a new phone — but the deeper reason that seam had to exist is this purchase-frequency mismatch. Apple and Google didn't out-design the car manufacturers. They noticed that the car's software layer was aging on a phone's clock while stuck in a car's purchase cycle, and they built a system that quietly imports the phone's fast refresh into the car's slow one. The car's Gutenberg layer — the screen, the CPU, the buttons — stays exactly as old as the car. The Semantic layer riding on top gets replaced every time the driver upgrades their phone, on a cycle the car manufacturer never has to manage and never gets credit or data from.

That's also why manufacturers can't easily fight it. Building an equally fast-refreshing infotainment system would mean either treating the head unit as a genuine consumable — a real replacement product, sold and serviced on a 2–3 year cycle nobody has built the retail channel for — or matching Apple and Google's OTA cadence on hardware that was speced and frozen at the car's design stage, years before it reached the showroom. Both are structurally hard for a company whose entire business, and entire feedback loop, runs on the car's clock, not the phone's. CarPlay doesn't just look better. It solves a consumable-mismatch problem the car industry never built the infrastructure to solve for itself, by borrowing a refresh cycle that belongs to a different industry entirely.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Google Says Welcome Home. Your Car Just Sold You a Home Button.](https://rinie.github.io/2026/08/01/finished-at-the-point-of-sale/) on the finished-at-sale mindset, [Why Does My "Smart" Coffee Machine Not Know My Usual Cuppa?](https://rinie.github.io/2026/07/31/smart-coffee-machine-usual-cuppa/) on within-product learning that has no excuse to be missing, and [Don't Erase the White Line. Don't Pour Concrete Either.](https://rinie.github.io/2026/07/27/dont-hide-the-fence/) on the seam that holds when your phone upgrades the infotainment.*
