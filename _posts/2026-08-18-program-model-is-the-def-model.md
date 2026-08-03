---
layout: post
title: "The Program Model Is the Def Model, and It's the One That Has to Move"
date: 2026-08-18
tags: [gutenberg-semantic, joel-spolsky, def-push, use-pull, user-model]
level: general
description: "Joel Spolsky drew the line in 2000: the user model is what a person already believes about how software works before they ever touch yours. The program model is what the programmer or designer actually built. When they conflict, Joel's rule is blunt — change the program model, because the user model is remarkably hard to move and the program model is the one thing still in your control."
---

[Joel Spolsky's essay from 2000](https://www.joelonsoftware.com/2000/04/11/figuring-out-what-they-expected/) draws a line this series has been redrawing under different names for months, and it's worth reading in his own terms before translating it. A new user, he writes, *"does not come with a completely clean slate."* They arrive with a *user model* — their existing mental picture of how the software will work, inherited from every other program they've used before yours. The software itself has a *program model* — what it actually does, encoded in the code and *"executed faithfully by the CPU."* Joel's whole essay rests on one line: *"if the program model corresponds to the user model, you have a successful user interface."*

The program model is the programmer's model, or the designer's — whoever built the thing decided what it actually does. In this series' terms, that's the Def model. The user model, formed independently and in advance, is the Use model. Joel is describing the Def/Use split without ever using those words.

---

## Joel's Rule: Move the Def Model

When the two don't match, Joel is unambiguous about which one gives way: *"if the mountain won't come to Muhammad … your best choice is almost always going to be to change the program model, not the user model."* Not the user's assumption. The thing the programmer built.

His reasoning is entirely practical, not a claim that the user is somehow right. His own example proves the opposite — a user who assumes Word-style picture embedding will work the same way in an HTML editor is simply wrong about how HTML works. The program model there is *true*; the user model is a false assumption carried over from different software. Joel argues you accommodate it anyway, because of what it would cost to fix the other side: *"users don't read manuals"* and *"they probably shouldn't have to."* A dialog box explaining the real behaviour fares no better — *"users don't read dialog boxes, either."* The user model, once formed, is *"remarkably hard"* to move by explanation. The program model is the one variable the Def still has open. So that's the side that moves.

---

## Finding the User Model Instead of Guessing It

Joel doesn't stop at the principle — he gives the practical method for identifying the user model rather than assuming it. *"Just ask them,"* he writes: describe the situation, then ask what they expect to happen, and watch for consensus rather than any one person's guess. His photo-album example is exactly this in practice — instead of arguing over where thumbnail files should be stored, ask a handful of users where they *think* the thumbnails will end up, and build the program model to match whatever answer keeps coming up. Five or six people are enough, in his account, because after that *"you start seeing the same results again and again."* The method isn't a focus group vetting a decision the Def already made. It's the Def going and finding the Use model that already exists, before building anything.

---

## Simple Models Win

Joel's closing rule of thumb is worth keeping as its own point: *"when people have to guess how a program is going to work, they tend to guess simple things, rather than complicated things."* His Excel example makes the cost of ignoring this concrete — two spreadsheet windows that *look* independent but are secretly glued together by an "invisible sheet" concept nobody would ever guess unprompted, so clicking one window unpredictably brings a different one forward. The program model was internally coherent. It just wasn't anything a user would ever arrive at on their own, and *"it's hard enough to make the program model conform to the user model when the models are simple. When the models become complex, it's even more unlikely."*

That's the practical shape of the whole argument: find the user model by asking rather than assuming, then bring the program model to it — not because the Def is obligated to defer, but because a nontrivial program model was never going to be guessed correctly in the first place, and the Def is the only side of the equation still free to change.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Architecture Astronauts](https://rinie.github.io/2026/08/12/architecture-astronauts/) on Joel Spolsky elsewhere in this series, and [Use-Specialisation](https://rinie.github.io/2026/08/08/use-specialisation/) on the Def keeping the pipe wide until the Use side narrows it.*

Source: [Figuring Out What They Expected](https://www.joelonsoftware.com/2000/04/11/figuring-out-what-they-expected/), Joel Spolsky, Joel on Software, April 11, 2000.
