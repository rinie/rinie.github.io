---
layout: post
title: "Kanban Already Had the Words: Push and Pull, Before This Series Borrowed Them"
date: 2026-08-13
tags: [gutenberg-semantic, def-push, use-pull, kanban, agile, backlog]
level: general
description: "This series' Def-Push and Use-Pull vocabulary didn't come from nowhere. Kanban had already named the same distinction, in project management terms, years before any of this: work assigned to a person is a push. Work grabbed from a shared backlog when someone's actually ready for it is a pull. The reasoning underneath both is identical."
---

This series has used Def-Push and Use-Pull as load-bearing vocabulary for months without stopping to credit where the underlying distinction actually comes from. Kanban had already named it, in plain project-management terms, in an [ordinary DZone Agile article](https://dzone.com/articles/dont-push-pull-work-items-for-better-results) from 2015: *"the difference between using a pull and a push system of work assignment is in the optimization of the way items are distributed among the team."*

The framing translates almost without adjustment.

---

## Assigned Work Is Def-Push. A Shared Backlog Is Use-Pull.

In a push system, work items get assigned to specific people up front, creating *"separate queues of tasks."* The person deciding what work exists and who does it — the Def — pushes the decision onto the Use side before the Use side is ready to act on it. If that person takes leave, gets overwhelmed, or simply hasn't gotten to it yet, the item sits in a private queue nobody else can see or touch, regardless of how urgent it's actually become.

In a pull system, work items sit in a shared backlog, and *"the items / tasks get moved as the work progresses"* only when someone is actually ready to grab the next one. The article's specific claim: *"if the tasks are queued in a common backlog, chances are that if an item is of a high priority, someone will pull it quicker than it would have happened while it was assigned to someone."* The Use side pulls exactly what it can act on, exactly when it's ready — [the same resolver principle](https://rinie.github.io/2026/08/08/use-specialisation/) this series keeps finding in DNS, npm, and SQL, five years earlier, applied to a kanban board instead of a database schema.

---

## The Same Failure Mode, Named in Kanban Terms

The push-system risk the article names is worth reading against everything this series has said about Def-Push resolvers elsewhere: *"a risk in having items assigned to specific people lies in losing grip on where specific tasks have landed and in having them stuck in queues while people take leave or get overwhelmed with work."* That's the identical shape as [a hardcoded resolver with no confirmation loop](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/) — work gets pushed to a destination and there's no mechanism to notice it stalled there, no last-known-good to fall back to, no signal that anything's wrong until someone goes looking.

The pull-system fix is the same fix this series keeps landing on: don't push the decision early and hope it was the right one. Leave the work — the DNS record, the dependency version, the task — in a shared, visible place, and let whoever's actually ready pull it, at the moment they're ready, based on current priority rather than a decision frozen at assignment time.

---

## Accountability Without Ownership

The article's point about collaboration is worth keeping too, because it answers an objection this series' resolver arguments sometimes invite: doesn't pulling instead of pushing dissolve accountability? *"Accountability is better perceived as regarding a complete working item, rather than a perfectly done set of not necessarily associated tasks."* Accountability in a pull system attaches to the outcome, not to whoever happened to be assigned the ticket. That's the same shift [a view over a raw table](https://rinie.github.io/2026/08/08/use-specialisation/) makes — the contract is what the consumer actually gets, not which specific internal column produced it. Ownership of a fixed queue isn't the same thing as accountability for a result, and conflating them is exactly what a push system does by default.

---

## Scalability Is a Pull-System Side Effect

One more point from the source worth keeping, because it generalises past project management: *"because of the same reasons, any necessary changes in the team size for specific projects can be done quicker, since team members have no long item backlogs to fulfil."* A pull system scales cleanly because nobody's holding a private, pre-assigned queue that has to be manually redistributed when the team changes shape. This is the same property [a real resolver gives a growing system](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) — add a new consumer, and it just starts pulling from the same shared source everyone else pulls from, no reassignment required, no coordination meeting to figure out whose queue the new person inherits.

None of this is a new discovery. It's the oldest version of an argument this series keeps making in newer clothes — DNS, npm, SQL, a coffee machine, a phone number. Kanban had it first, in the plainest possible terms, describing nothing more exotic than how a small team decides who does what next.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Use-Specialisation](https://rinie.github.io/2026/08/08/use-specialisation/) on staying wide until the point of pull, [It Is Always DNS](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/) on the confirmation loop a push system lacks, and [Borg's Arrogance](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) on resolvers that scale versus queues that don't.*

Source: [Don't Push! Pull Work Items for Better Results](https://dzone.com/articles/dont-push-pull-work-items-for-better-results), Anna Majowska, DZone Agile, July 20, 2015.
