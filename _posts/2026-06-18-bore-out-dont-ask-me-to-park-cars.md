---
layout: post
title: "I Didn't See the Bore-Out Coming. Don't Ask Me to Park Cars."
date: 2026-06-18
tags: [def-push, use-pull, bore-out, marvin, capability, feedback]
level: general
description: "Bore-out is not burnout. Burnout is too much demand. Bore-out is too little meaning — the slow accumulation of capability misallocation that nobody notices until it is well advanced. Marvin the Paranoid Android had a brain the size of a planet and was assigned to park cars. The missing piece was not motivation. It was a resolver between capability and task."
---

You did not see it coming. That is the thing about bore-out — it arrives slowly, one reasonable assignment at a time, until one day you are fixing variant #847 of the same underlying bug and realise that you have been doing this for two years and the underlying bug has not moved.

Burnout you can see approaching. It has a shape — too much, too fast, too long, the demands exceeding the capacity. Bore-out has no shape. It is the absence of shape. The gradual removal of challenge, meaning, and the sense that the work is moving somewhere. Each individual task is reasonable. The accumulation is the problem.

*"Here I am, brain the size of a planet, and all they ask me to do is park cars."*

Marvin the Paranoid Android, from Douglas Adams' Hitchhiker's Guide to the Galaxy, is the canonical bore-out case. Genuine People Personality. Intelligence of extraordinary depth. Assigned to open doors, park cars, and stand around waiting. The capability is real. The task is not wrong — the cars do need parking. The mismatch is structural, not personal, and nobody designed it maliciously. It just happened, one reasonable assignment at a time.

---

## The Slow Accumulation

Bore-out does not arrive in a single bad decision. It accumulates through a series of individually defensible ones.

The project needed someone reliable for the legacy system maintenance. You were available and good at it. The sprint needed someone to handle the backlog of small bugs. You cleared them efficiently. The team needed someone who knew the old codebase well enough to investigate the intermittent production issue. You were the obvious choice.

Each assignment was reasonable. Each one made sense in the sprint planning meeting. None of them, individually, was the problem.

The problem is that the assignments accumulated without a corresponding accumulation of the things that made the work meaningful: the architectural problem that needed solving, the new capability that needed building, the hard question that needed thinking through. The Gutenberg tasks (fix the bug, close the ticket, maintain the system) kept arriving. The Semantic work (design the solution, understand the domain, grow the capability) did not.

Marvin was not assigned to park cars because anyone decided he should spend eternity doing it. He was assigned to park cars because the cars needed parking and he was there. One reasonable decision. Thirty-seven million years later, still parking cars.

---

## The Missing Resolver

The manager in this story is not the villain. The sprint planning process is not the villain. The backlog is not the villain.

The missing piece is a resolver — a mechanism that maps the semantic capability of the people doing the work to the semantic requirements of the tasks being assigned. Without the resolver, the Gutenberg tasks (the tickets, the bugs, the maintenance items) dominate the visible work surface. The Semantic work (the architectural thinking, the capability development, the hard problems) is invisible in the sprint board.

The resolver looks like:

- The retrospective that actually surfaces "I have been on legacy maintenance for six months and I need a different kind of work"
- The one-to-one where the manager asks "what would you work on if the backlog were yours to prioritise?"
- The technical debt budget that makes architectural work a first-class item rather than a stolen hour between tickets
- The 20% time that is genuinely protected rather than quietly reclaimed when the sprint is full
- The role description that specifies not just what the engineer will do but what they will *not* be asked to do indefinitely

Without the resolver, the engineer fixes bug #847. Then #848. Then #849. The capability atrophies from disuse. The bore-out advances. Marvin gets another car.

---

## "I'm Not Getting You Down at All, Am I"

In *The Restaurant at the End of the Universe*, Zaphod Beeblebrox greets Marvin with cheerful enthusiasm after Marvin has been waiting in a parking lot for thirty-seven million years.

Marvin's response: *"I'm not getting you down at all, am I."*

This is the most devastating possible understatement. Thirty-seven million years. A brain the size of a planet. Parking cars. And the response to Zaphod's forced cheerfulness is not anger, not complaint, not a demand for better treatment. It is a dry observation about the gap between how the conversation is being performed and how it is actually going.

The manager who opens the one-to-one with "great to see you, how's the team doing!" while the engineer has been on legacy maintenance for eight months is Zaphod. Not malicious. Not unaware that something might be wrong. Just performing the interaction in the register that feels appropriate, without a feedback mechanism that would tell them the register is wrong.

The engineer who says "fine, the tickets are moving" when they mean "I haven't done anything interesting in two years" is Marvin. Technically accurate. Completely uninformative. The resolver between what is happening and what is being communicated is missing on both sides.

The Gemba walk for bore-out is sitting with someone while they work and watching what lights them up and what they do mechanically. The sticky note equivalent is the GitHub comment that is unusually detailed for a routine ticket — someone finding something interesting in the work they were not expected to find interesting. The workaround equivalent is the side project the engineer runs in their own time on problems the job has stopped giving them.

---

## The Doors That Sigh

The Sirius Cybernetics Corporation solved the bore-out problem in the opposite direction.

Their elevator doors were programmed with a cheerful and sunny disposition. It was their *pleasure* to open for you. Their *satisfaction* to close again with the knowledge of a job well done.

The door does not have a brain the size of a planet. It has no unmet potential, no atrophying capability, no bore-out accumulating. It is a mechanical panel that opens and closes. The Genuine People Personality was added by the Def (Sirius Cybernetics) without consulting the Use (the person waiting for the lift) because personality was considered a selling point.

Marvin and the door bracket the complete capability misallocation spectrum:

- **Give personality to things that don't need it** (the door): Def-Push imposing a Semantic layer on a Gutenberg artifact that has no use for it
- **Ignore the actual Semantic needs of things that have them** (Marvin): Def-Push assigning Gutenberg tasks to a Semantic capability without a resolver between them

Both failures come from the same source: the Def deciding what the Semantic layer should be without the Use signal. The door did not ask for feelings. Marvin did not ask for parking duty. Nobody asked either of them.

The GPP in software is the door: the chatbot that says "I'm happy to help!" regardless of whether it helps, the assistant that generates "Great question!" before a wrong answer, the onboarding flow that says "Let's get you set up!" while taking you through seventeen steps you did not want to take. The Semantic layer (the enthusiasm) imposed on the Gutenberg interaction (the transaction) without a feedback loop to check whether the enthusiasm is appropriate.

The bore-out engineer is Marvin: the Semantic capability (the thinking, the design sense, the domain knowledge) assigned indefinitely to Gutenberg tasks (the tickets, the maintenance, the parking) without a resolver to surface the mismatch.

---

## The Structural Fix

Bore-out is not fixed by motivational posters. It is not fixed by "bring your whole self to work." It is not fixed by the team building day. These are Genuine People Personalities applied to a structural problem — the door sighing with satisfaction at closing does not fix the fact that Marvin is still parking cars.

The structural fix is the resolver: the mechanism that surfaces the gap between capability and task before it becomes a thirty-seven million year problem.

For the engineer: name the mismatch. "I have been on Gutenberg tasks for six months. I need Semantic work." Not a complaint — a Use signal. The feedback loop requires someone willing to send the signal and someone willing to receive it.

For the manager: go to the Gemba. Watch what lights people up and what they do mechanically. Read the sticky notes. The engineer who has been on legacy maintenance for eight months and still writes unusually thoughtful commit messages is Marvin parking cars while designing a better parking system in their head. The capability is there. The resolver is missing.

For the organisation: make the Semantic work visible. Technical debt is Semantic debt — the gap between what the system does and what it should do. If it does not appear in the sprint, it does not get worked on. If it does not get worked on, the engineers who could work on it park cars instead.

Marvin's depression is proportional to the capability. The frustration is the Use signal. The correct response is not to adjust Marvin's Genuine People Personality settings until he seems happier about the parking. It is to build the resolver that connects his brain the size of a planet to a problem worth thinking about.

The bore-out did not arrive in a single bad decision. It will not be fixed in a single good one. But it starts with noticing the mismatch — which is what the Gemba walk is for, and what the sticky note was trying to say.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Going to the Gemba](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on surfacing the Use signal where it lives, [The Complaint Department Has Been Transferred to Another Dimension](https://rinie.github.io/2026/06/16/complaint-department-transferred/) on what happens when the feedback loop closes entirely, and [42: You Still Need a Towel at the Waterline](https://rinie.github.io/2026/06/25/42-towel-at-the-waterline/) — coming soon.*
