---
layout: post
title: "Every 'Where Is the Close Button?' Is a Bug"
date: 2026-06-19
tags: [ux, def-push, use-pull, krug, interface, cognitive-tax]
level: general
description: "Steve Krug's 'Don't Make Me Think' is the Use-Pull manifesto for interface design. Every moment of hesitation — 'wait, where is the close button?' — is the interface failing the user. The cognitive tax is not a feature. It is a debt the designer chose not to pay."
---

Steve Krug's *Don't Make Me Think* (2000, revised 2014) contains a principle so obvious that it should not need stating, and so consistently violated that it needs stating on every project, in every sprint, in every design review, indefinitely:

**The interface should not make the user think.**

Not "should not make the user think too much." Not "should minimise unnecessary cognitive load." Should not make the user think. Every moment of hesitation — every "where is the close button?", every "wait, is this the right button?", every "I think I clicked the wrong thing" — is the interface failing. Not the user. The interface.

This is Use-Pull stated as a design rule. Observe what users actually do. Remove the friction between their intent and the action. Let the Use signal shape every element — its position, its label, its size, its affordance. The interface that requires no thought is not simple. It has absorbed all the complexity that would otherwise land on the user.

---

## The Cognitive Tax

Every unnecessary question the interface forces the user to answer is a tax on the user's cognitive budget. The budget is finite. Every cent spent on navigating the interface is a cent not spent on the actual task the user came to do.

*"Where is the close button?"* — tax.
*"Wait, does this button save or discard?"* — tax.
*"Is this clickable?"* — tax.
*"Did that actually submit?"* — tax.
*"Why is this option greyed out?"* — tax.
*"I didn't mean to click that"* — tax plus penalty.

None of these taxes are free. They accumulate across every interaction, every session, every day the user uses the system. The accountant who is uncertain about three interface elements per task, across forty tasks per day, across two hundred working days per year has paid sixty times forty times two hundred cognitive micro-taxes. The interface designer who charged those taxes is not present to pay them.

The cognitive tax is always paid by the user and never by the designer. This asymmetry is the structural reason interfaces accumulate friction over time — the people who add features pay no cost for the cognitive overhead those features introduce, while the users pay it on every interaction.

---

## The Disappearing Close Button

The GPP that cannot be dismissed is the purest violation of Krug's principle in contemporary software.

The chatbot appears. The user does not want the chatbot. The user looks for the close button. The close button is not where close buttons usually are. The user looks harder. The close button is not there. The user must either interact with the chatbot or work around it.

This is not an accident. Someone in a meeting decided the chatbot should stay visible, approved the removal of the exit, and shipped it. The Def was confident. The user was not consulted.

The tax imposed: find the close button (fail), accept the chatbot is staying (cognitive acceptance), adjust workflow to work around it (ongoing cost), feel slightly violated by the experience (emotional overhead not on any spreadsheet).

The benefit to the user: none.
The benefit to the platform: chatbot engagement metrics look better.

This is the disappearing close button as a business model. The user pays the cognitive tax. The platform collects the engagement data. The GPP says "I'm here to help!" while making it impossible to say "no thank you."

Krug's rule is violated at its most basic level: the interface is not serving the user's intent. It is serving the platform's goal of keeping the GPP visible. The "where is the close button?" is not a bug to the platform. It is working as designed.

---

## The Jony Ive Corollary

Beautiful design can impose cognitive tax. This is the uncomfortable truth that Krug's principle forces.

The iOS 7 redesign removed visual affordances that signalled "this is tappable." The flat design was aesthetically coherent and genuinely beautiful. It was also less immediately usable for people who had learned to use iOS under the previous convention. The button that looked like a button no longer looked like a button. The cognitive tax of "is this interactive?" was paid by every user who had to relearn the visual grammar of the interface.

The ultra-thin MacBook with one USB-C port is the hardware version. Beautiful object. Correct aesthetic statement about the future of connectivity. Tax: every user who needs to connect anything must carry a dongle and make a routing decision before they can work. The cognitive overhead is distributed to every workflow, every day, forever.

The aesthetic was the Def's priority. The user's cognitive budget was secondary. The tax was charged without consultation.

This is distinct from the Inmates problem — the engineer who cannot see the confusion because nothing is confusing to them. Ive could see the confusion. He saw it as a transitional cost the user should pay on the way to a better future. The Krug rule says: the transitional cost is not yours to assign. The user's cognitive budget belongs to the user.

---

## What Does Not Make You Think

The interface that does not make you think is not minimal or stripped down. It is precisely calibrated.

Every element is where the user expects it to be — not where the designer found it interesting to put it, not where it fits the visual grid, not where it matches the brand guidelines. Where the user expects it. This requires knowing where users expect things to be, which requires watching users use the interface, which is the Gemba walk applied to design.

Every label says what the thing does — not what it is called in the codebase, not what the product manager named the feature, not what sounded exciting in the meeting. What it does. From the user's perspective, not the system's.

Every action has a visible outcome — the button that was clicked looks different after clicking, the form that was submitted confirms submission, the item that was deleted is visibly gone. The user should never wonder "did that work?" because the interface answers the question before it can be asked.

Every error says what happened and what to do next — not `ERROR_4721`, not `An unexpected error occurred`, not `Something went wrong`. What happened, in the user's terms. What to do about it.

None of these are technically difficult. All of them require genuinely listening to the Use signal — watching real users, reading the sticky notes, going to the Gemba — rather than assuming the designer's mental model matches the user's.

---

## The Use-Pull Interface

Krug's principle is the operational version of Use-Pull applied to design:

The Def (the designer, the product manager, the developer) creates the interface. The Use (the person who has to use it) encounters it. The gap between the Def's model of what the interface should require and the Use's experience of what it actually requires is the cognitive tax.

The Use-Pull interface closes that gap continuously:
- Watch real users encounter the interface (Gemba)
- Observe where they hesitate, where they fail, where they pay the tax
- Remove the friction between intent and action
- Ship the change
- Watch again

The Def-Push interface is designed once, shipped, and defended. The "where is the close button?" is the user's problem. The confusion is attributed to insufficient training. The cognitive tax is the cost of using a sophisticated tool.

The users who are confused are not the problem. The interface that confused them is the problem.

Every "where is the close button?" is a bug. File it, fix it, watch whether the fix worked. That is the whole discipline.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Going to the Gemba](https://rinie.github.io/2026/06/17/gemba-waterline/) on finding the Use signal where it lives, [I Didn't See the Bore-Out Coming](https://rinie.github.io/2026/06/18/bore-out-park-cars/) on what happens when the signal is not heard for long enough, and [Ten Users Saying It Sux Means It Sux](https://rinie.github.io/2026/06/02/ten-users-saying-it-sux/) on the population math behind the Use signal.*
