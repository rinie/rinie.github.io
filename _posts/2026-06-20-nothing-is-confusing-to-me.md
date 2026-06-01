---
layout: post
title: "Nothing Is Confusing to Me: The Inmates Are Running the Asylum"
date: 2026-06-20
tags: [ux, def-push, use-pull, cooper, proximity-blindness, interface]
level: general
description: "Alan Cooper's 'The Inmates Are Running the Asylum' names the failure mode precisely: the engineers who built the software are now designing the interface. Not malicious. Genuinely blind to the gap between their own mental model and the user's. Proximity creates blindness. The fix is not smarter engineers — it is exposure to actual users."
---

In the [Gutenberg/Semantic model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) the semantic layer has two sides. The **Def side** — the producers, the architects, the engineers, the people who built the system and know it completely. The **Use side** — the users, the clients, the people the system is supposed to serve.

The problem named by Alan Cooper in his 1999 book *The Inmates Are Running the Asylum* is not that the Def side is malicious. It is that proximity to the Def makes the Use side invisible. The people closest to the system are the worst possible designers of its interface — not because they are incompetent but because nothing is confusing to them.

The programmer who wrote the error message that says `NullPointerException` was not trying to confuse anyone. They just never considered that the user is not them. The developer who designed the 47-step installation wizard thought they were being thorough. The engineer who put twelve options in a dropdown thought they were being comprehensive. The UI that made perfect sense to the person who built it is the UI that sends support tickets from everyone else.

---

## Proximity Creates Blindness

Cooper's insight is precise: the mental model of the person who built the system is so complete, so internalised, so automatic, that gaps in the interface are invisible to them. They cannot see what is confusing because nothing is confusing. The path through the interface that confuses every new user is transparent to the engineer who designed it — they know where the button is, they know what the error means, they know which menu contains the option. They have always known. They cannot unknow it.

This is distinct from the failure named in the [previous post](https://rinie.github.io/2026/06/19/where-is-the-close-button/) — the designer who knew about usability but chose brand over user. The inmates are not making a choice. They are experiencing a genuine inability to see from the user's perspective. The failure is not arrogance. It is the natural consequence of expertise.

The expert skier cannot remember what it felt like to not know how to turn. The expert programmer cannot remember what it felt like to not know what a null pointer is. The expertise is real and valuable. The blindness is its shadow.

---

## The Two Steves and the Feedback Threshold

The difference between engineers who produce usable interfaces and engineers who produce confusing ones is not intelligence or care. It is the feedback threshold — how loud the Use signal has to be before it reaches the Def.

**Steve Wozniak** had a low threshold. He went to the Homebrew Computer Club. He watched hobbyists use his machines. He asked questions. He listened before the design was locked in. The Use signal arrived early and cheaply — a conversation, an observation, a question from someone who was confused. The Apple II had expansion slots because Woz heard "I want to connect things to this" before the case was closed.

Woz was not immune to Dancing Bears. Alan Cooper uses the term for features that impress in a demo but serve nobody in daily use — the bear that dances is impressive, the bear that dances is not useful. Woz genuinely believed the Segway would conquer the world. Invested in it. Rode one everywhere. The Use signal (people finding it embarrassing in public, cities banning it from pavements, the price making it inaccessible) was there. For once his threshold was higher than usual. The Segway did not conquer the world. It became a vehicle for mall security guards and tourist groups.

Mostly harmless. Woz was not a jerk about the Segway. He did not impose it on anyone. He believed in a thing that did not work out, moved on, remained delighted by technology. The error was proportional. The humility remained. The threshold recalibrated.

**Steve Jobs** killed the Dancing Bears of engineers ruthlessly — every feature that added complexity without serving the user in daily use was cut. No feature creep. No "but it's technically impressive." If it did not serve the person holding the phone, it was gone. His threshold for engineering Dancing Bears was remarkably low.

But he was blind to the Dancing Bears of designers — the features that looked beautiful in a presentation, that impressed in a keynote, that made reviewers gasp, but that created daily friction for the person living with the product. The butterfly keyboard danced beautifully on stage. The single-port MacBook was a design statement. The Mac Pro trash can was stunning in a photo. All Dancing Bears. All imposing daily cognitive tax on the users who actually lived with them.

The distinction is precise:
- **Engineering Dancing Bears** — technically impressive, nobody asked for it, adds complexity. Jobs saw these clearly and cut them.
- **Design Dancing Bears** — visually impressive, makes great keynote material, creates daily friction. Jobs could not see these because they were his tribe's output.

Jobs had a high threshold for designer Dancing Bears because the designers were his tribe. The feedback that the butterfly keyboard was unreliable had to become a PR crisis before it broke through. Not because he did not care about users — because the aesthetic conviction was load-bearing. Admitting the keyboard was wrong meant admitting the statement it made was wrong.

**Jony Ive** had a high threshold shaped by the same aesthetic conviction. The butterfly keyboard produced genuine user suffering for years before the feedback broke through. The threshold was set by the identity investment, not by indifference.

The inmates have threshold zero — no feedback reaches the designer because they cannot see the confusion. Not because they have stopped caring. Because proximity has made the confusion invisible.

**The calibrated threshold — the Woz/Krug position:** low enough that small friction is worth investigating, high enough that every random opinion does not derail the design. The weak link willing to learn. The designer who asks "what did I miss?" when a user hesitates, rather than "why does the user not understand this?"

---

## CarPlay and Android Auto: Beautiful Interfaces for the Wrong Context

CarPlay and Android Auto exist because the native car infotainment systems were built by automotive engineers designing for automotive engineers. The menu structures mirrored the engineering team's internal taxonomy. Features were buried three levels deep because that was how the software was architected. Error messages described the system state rather than the driver's situation.

The inmates were running the in-car interface. The result was systems so confusing that drivers were looking away from the road for dangerous durations trying to find basic controls.

CarPlay and Android Auto are Apple and Google saying: we have watched users struggle with interfaces, we know how to design for touch and glance, let us put our semantic layer over your Gutenberg hardware. The result is better — because Apple and Google had actually watched users fail, had accumulated the Use signal that automotive engineers never collected.

But CarPlay still carries the inmates problem one layer up. The interface conventions it imports were designed for a user sitting still, with full attention, holding the device in two hands. The driving context is completely different: eyes on road, one hand available, cognitive load high, interaction window of one or two seconds. The inmates running the in-car interface were replaced by a different set of inmates — smartphone interface designers imposing their mental model on a driving context they had not inhabited.

The Woz solution would have been: talk to drivers. Watch them drive. Ask what they need to do with one glance and one tap. Design from that constraint outward, not from the smartphone interface inward.

---

## cmd, PowerShell, and the Power User Trap

The inmates problem has a variant that is easy to miss: the power user who has crossed the expertise threshold and can no longer see what was confusing about the interface before they learned it.

`cmd` is the Woz solution for the Windows command line: commands close to what they do (`dir`, `copy`, `move`, `del`), syntax transparent enough that a user who has never used a command line can make a reasonable guess. Not powerful. Not elegant. Honest about what it is.

PowerShell is the inmates solution: a rich, consistent, powerful object pipeline model designed by people who knew exactly what was wrong with cmd and fixed all of it. Beautifully consistent — verb-noun cmdlets, real scripting, proper error handling, pipeline objects instead of text streams. The inmates were not wrong about what was wrong with cmd. They were wrong about who the user was.

`Get-ChildItem -Path C:\ -Recurse -Filter *.txt | Where-Object { $_.LastWriteTime -gt (Get-Date).AddDays(-7) }` is a thing that a PowerShell expert reads in five seconds. For the user who wanted `dir`, it is a cognitive wall. The power user who designed it cannot see the wall because they climbed it years ago and forgot it was there.

Bash, on the Unix side, has the same accumulated complexity: quoting rules that change meaning based on context, `$IFS` silently altering word splitting, decades of layered conventions that are obvious to anyone who has used Unix for ten years and opaque to everyone else. The inmates ran the shell and produced something powerful and inscribed with the mental models of every decade of power users who improved it.

The preference for cmd over PowerShell is not a statement that PowerShell is worse. It is a statement about which users are being served. cmd was designed close to the Use side — slightly clumsy, transparent, honest. PowerShell was designed from the Def side outward — powerful, consistent, demanding.

---

## All Inmates Are Equal. Some Are More Equal Than Others.

Orwell's line from Animal Farm applies precisely. The revolution starts to serve everyone. The pigs end up running the farm for themselves. Each generation more convinced their model is correct. The Use signal — the hungry animals, the confused users, the driver looking away from the road — reframed as a management problem rather than a design failure.

The inmates who learn from feedback become less equal over time. They drift toward the users, away from the Def. Each piece of friction they witness recalibrates their threshold a little lower. The expertise remains. The blindness recedes.

The inmates who persist become more equal — more convinced, higher threshold, more insulated from the Use signal that would correct them. The mental model hardens. The interface stops changing. The support tickets accumulate.

The fix is not smarter engineers. The fix is exposure — real users, real confusion, watched in real time without helping. The Gemba walk for interface design: sit with someone who has never used the system, watch them use it without intervening, note every hesitation, every wrong tap, every "where is the close button?" The five hours of watching three users fail is worth more than three months of internal design review.

Not because three users are statistically significant. Because watching someone hesitate once, out loud, in front of you, destroys the confident assumption that the interface is obvious. The threshold recalibrates. The blindness lifts a little.

**Learn from feedback or persist as an inmate.** That is the choice. Not a moral choice — a design one. The interface that keeps confusing users is the one whose designers never watched a user be confused.

The user does not hold it the wrong way. The designer who thinks they do has stopped learning.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) and the UX trilogy. Related: [Every 'Where Is the Close Button?' Is a Bug](https://rinie.github.io/2026/06/19/where-is-the-close-button/) on Krug's principle and the cognitive tax, [Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on watching the Use signal where it lives, and [Muddy Water(line)s, Shady User Experience](https://rinie.github.io/2026/06/21/muddy-waterlines-sux/) on when the interface is wrong by design rather than by blindness.*

---

## A Note for Anyone Who Skipped to the End

Skipping to the end of a long post is a completely legitimate Use pattern. The person who reads the conclusion first and decides whether the argument is worth their time is using the post correctly — the index before the book, the abstract before the paper. If you are here without reading the rest: welcome. Here is the summary.

The Def side is not the villain. The producer, the architect, the engineer, the designer — without the Def side nothing gets made. The problem is not being on the Def side. It is staying exclusively on it. Never crossing to the Use side to see what the thing is actually doing to the people using it.

The darkness is not in the Def. It is in the distance. The Def that has never visited the Use side cannot see what it is producing from the user's perspective. Not because it is bad — because it has not looked.

Woz crossed regularly. Low threshold, Homebrew Club, expansion slots before the case was closed. Jobs crossed selectively — brilliantly for consumer products, less so for developers and enterprise users. Ive crossed rarely for anything that conflicted with the aesthetic vision. The inmates do not cross at all, because nothing is confusing to them.

The fix is not being less of an engineer or less of a designer. It is crossing more often. Watching users. Reading the sticky notes. Going to the Gemba. Letting the Use signal reach the Def before the design is locked in rather than after the PR crisis.

The Def side that crosses regularly stays calibrated. The corrections are small. The Dancing Bears get spotted early. The waterline stays clean.

And if the jerks read this far: the post was about you too. Mostly harmless to admit it.
