---
layout: post
title: "Every 'Where Is the Close Button?' Is a Bug"
date: 2026-06-19
tags: [ux, def-push, use-pull, krug, interface, cognitive-tax]
level: general
description: "Steve Krug's 'Don't Make Me Think' is the Use-Pull manifesto for interface design. Every moment of hesitation is a design failure. Every 'where is the close button?' is a bug. The interface that requires no thought is not simple — it is deeply considered, shaped entirely by the Use signal of what people actually do."
---

In 2000, Steve Krug published a book called "Don't Make Me Think." The title is the argument. The interface that works is the one that requires no thought — not because it is simple but because it has absorbed all the complexity that would otherwise land on the user.

Every "where is the close button?" is a bug.
Every "wait, how do I get back?" is a bug.
Every "I didn't mean to click that" is a bug.
Every animation that delays the action the user wanted to take is a bug.
Every confirmation dialog for a reversible action is a bug.

These are not edge cases. They are the ordinary friction of ordinary interfaces, accumulated one design decision at a time, paid for by users in units of cognitive tax they did not consent to spend.

---

## The Cognitive Budget

Every user arrives with a finite cognitive budget. It is not the same every day — it depends on how tired they are, how familiar the interface is, how much they care about the outcome. But it is always finite, and every interaction either spends it or respects it.

The interface that respects the cognitive budget routes the user's attention toward their actual goal — the thing they came to do — and away from the interface's own machinery. The back button is where they expect it. The confirm button is the primary action. The error message says what went wrong and how to fix it, not what the system state was when it went wrong.

The interface that spends the cognitive budget routes the user's attention toward itself. "Where is the close button?" is not a question about the user's goal. It is a question about the interface's design decisions. The cognitive budget is being spent on navigating the interface rather than accomplishing the task. The user is thinking about the tool rather than the work.

Krug's principle is not "make it simple." Simple interfaces are often over-designed in the other direction — so stripped of information that the user cannot find anything. Krug's principle is: direct the cognitive effort toward the task, not toward the interface. Make the interface transparent. Make the path to the goal obvious without making it feel obvious, because the effort of making it obvious is the designer's job, not the user's.

---

## The Disappearing Close Button

The disappearing close button is the clearest current example of cognitive tax imposed by design, not by necessity.

The chatbot appears in the corner of the screen. The user does not want it. The user looks for the close button. There is no close button. There is a minimise button that makes it slightly smaller. There is an X that opens a feedback dialog before dismissing it. There is no clean exit.

This was designed. Someone in a meeting decided that the chatbot should remain visible and approved the removal of the clean exit. The Def was confident: the chatbot is useful, users will come to appreciate it, reducing friction to dismissal will increase engagement. The Use signal — the user who does not want the chatbot and now has to work around it — was not in the meeting.

The cognitive tax: the user has to decide what to do about the chatbot. Then try the close button. Then discover it does not clean-close. Then decide whether to minimise or give feedback or just ignore it. Then continue trying to do the thing they came to do with the chatbot occupying a corner of their attention.

The user's goal has not advanced. The interface has extracted several seconds of cognitive budget and several small frustrations. The Def got what it wanted — the chatbot stayed visible. The Use paid for it.

Krug would call this exactly what it is: a bug. Not a feature with a difficult trade-off. A bug. The interface made the user think about the interface instead of their goal. That is the failure condition.

---

## Beautiful Design Can Still Be Wrong Design

The disappearing close button is usually not ugly. It is often beautifully designed — smooth animations, considered typography, a colour palette that matches the brand. The aesthetic is correct. The usability is not.

This is the distinction that the Don't Make Me Think principle makes precise. Jony Ive's designs were often aesthetically extraordinary and simultaneously usability failures:

**The ultra-thin MacBook** — one USB-C port. Beautiful object. The user who needed to charge and connect an external drive simultaneously had to buy a dongle. Every time. The cognitive tax: remember the dongle, find the dongle, connect the dongle, accept that the beautiful laptop now has a protrusion. Not a problem for the user who had no peripherals. A recurring friction for everyone else.

**The butterfly keyboard** — the thinnest possible key travel, in service of the thinnest possible laptop. Beautiful. Every keypress slightly wrong, slightly unreliable, slightly more cognitive attention required than a keyboard should need. "Did that letter register?" is a question the user should never have to ask. It is a bug, even when the keyboard is beautiful.

**iOS 7's flat design** — removed visual affordances that signalled "this is tappable." Beautiful. Users tapped things that were not tappable and missed things that were, because the visual signal had been removed in the name of aesthetic consistency. The cognitive tax: learn which things are tappable through trial and error rather than visual recognition.

In each case the aesthetic vision was real and the capability was genuine. The usability failure was not ignorance — it was a choice to prioritise the visual over the functional, the designer's aesthetic conviction over the Use signal of the user encountering the interface for the first time.

Beautiful design that makes the user think is still wrong design. Krug does not care how it looks. Krug cares whether "where is the close button?" was the user's experience.

---

## The Threshold

The Don't Make Me Think failure is not always malice or indifference. Often it is a threshold problem — the designer's feedback loop is calibrated to hear strong negative signal but not the ordinary friction of ordinary use.

A PR crisis about butterfly keyboards gets heard. A million individual users quietly adapting their typing style to compensate for unreliable key travel does not. The strong signal breaks through the threshold. The ordinary friction does not.

Krug's solution is usability testing — watching real users encounter the interface without helping them. Five users, five hours, more insight than a month of internal design review. Not because five users are statistically significant. Because watching someone say "where is the close button?" once, out loud, in front of you, recalibrates the threshold immediately. The friction that seemed minor in the design document becomes visible and urgent when you watch someone pay it.

The threshold problem is also why the Gemba walk matters. Going to where users actually use the system — the sticky notes, the workarounds, the undocumented shortcuts — surfaces the ordinary friction that never reaches the strong-signal threshold. The users who adapted do not complain. The sticky notes do.

---

## The Interface as a Use-Pull Contract

Krug's principle is Use-Pull interface design stated as a rule. The interface is a contract between the designer and the user. The designer's side of the contract: I will make the path to your goal obvious without making you think about the path. The user's side: I will tell you when the path is not obvious, through my hesitation, my errors, and my "where is the close button?" moments.

The Def-Push interface breaks the contract unilaterally: the designer decides the path, presents it with confidence, and treats the user's hesitation as a training problem rather than a design signal. "The interface is correct, the user needs to learn it." The familiar refrain. The wrong direction of causality.

The weak link willing to learn reads the hesitation as signal. "The user hesitated here — what is ambiguous about this element?" The interface improves. The cognitive tax decreases. The "where is the close button?" moments reduce.

Not because the user learned the interface. Because the designer learned the user.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) and the informal UX trilogy. Related: [Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on observing the Use signal where it lives, and [Ten Users Saying It Sux Means It Sux](https://rinie.github.io/2026/06/02/ten-users-saying-it-sux/) on the population math behind the Use signal.*
