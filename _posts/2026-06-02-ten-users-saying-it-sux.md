---
layout: post
title: "Ten Users Saying It Sux Means It Sux: Population Scaling and the Def-Push Distortion"
date: 2026-06-02
category: "UI & UX"
tags: [ux, feedback-loops, use-pull, product-management, scaling]
depth: conceptual
---

This is part of a series. The [Def-Use split](https://rinie.github.io/2026/05/16/def-use-lean-pull/) established that the gap between what authors define and what users actually need is where most waste lives. This post adds a statistical dimension: the people giving you feedback are not a random sample of your users, and the people defining your product are not a random sample of humanity. Both distortions push in the same direction — away from the user who does not sux.

---

## The 90/9/1 Rule You Already Know

In 2006, Jakob Nielsen described what he called the 90-9-1 rule for online communities: roughly 90% of users are lurkers who never contribute, 9% contribute occasionally, and 1% account for most of the content and activity. The numbers vary by platform and community but the shape is consistent — participation follows a power law, with the vast majority of users invisible to the system that serves them.

Nielsen's rule is about *participation rate*. The silent 90% are not disengaged — they are reading, using, benefiting. They just do not say anything. This matters enormously for product design: your most visible users (the 1% who post, comment, and complain loudly) are not representative of your actual user base. Optimising for the vocal 1% at the expense of the silent 90% is the most common and least noticed form of Def-Push.

This post adds a different dimension to the same observation: not just how many users speak, but what the behaviour distribution looks like across the full population — and how that distribution distorts when it reaches the Def-Push side.

---

## The User Population: 90/9/1 by Behaviour

Across any sufficiently large user population, the behaviour distribution is roughly:

- **~90%** — reasonable people trying to accomplish something. They have a goal, they use your product to achieve it, they move on. If something does not work they try again, work around it, or quietly leave. They do not complain. They do not praise. They are the silent majority whose needs define whether your product actually works.

- **~9%** — frustrated, vocal, occasionally rude. Crucially: they are usually frustrated *because something actually failed them*. The complaint may be phrased badly. The tone may be aggressive. But the underlying signal — this did not work — is almost always real. Dismissing the 9% because they are unpleasant is how you miss the bug report inside the complaint.

- **~1%** — genuine bad actors. Trolls, competitors, people testing edge cases with malicious intent. This population exists. It is small. It does not represent your users.

The rounding error is intentional. The exact numbers matter less than the shape: the overwhelming majority of your users are reasonable people trying to accomplish something, and most of them will never tell you when something fails.

---

## What Reaches the Def Side Is Not a Random Sample

Here is where it gets structurally important. The feedback that reaches the people building your product is not drawn randomly from the 90/9/1 population. It is self-selected in a specific way:

- The **9%** have the most motivation to complain — something failed them, they are frustrated, they want it fixed
- The **1% saints** (the constructive end of the 1%) have the most motivation to help — they care about the product, they want to improve it
- The **silent 90%** rarely say anything unless the barrier to feedback is extremely low

This means your bug tracker, your support queue, your app store reviews, and your user research sessions are all systematically over-representing the frustrated 9% and the engaged 1%, and under-representing the silent 90% who are the majority of your actual users.

The result is a feedback channel that looks like: 50% neutral, 40% negative, 10% constructive. This does not mean your product is terrible. It means you are seeing the self-selected complainers and helpers, not the silent majority who are fine — or who silently left without telling you why.

---

## Ten Independent Users Is a Bug Report

This is the practical implication of the population math. One user saying something sux might be noise — the frustrated 9% in action, a bad day, a misunderstanding. But ten independent users, unknown to each other, converging on the same complaint? That is signal regardless of how it is phrased.

The probability that ten uncoordinated people independently generate the same false complaint about the same specific thing is vanishingly small. If ten users all say the same flow is confusing, the flow is confusing. If ten users all report the same error, the error is real. The phrasing may be wrong. The diagnosis may be wrong. The underlying experience — this did not work for me — is almost certainly real.

The burglar edge case is the correct limit: if 100% of your audience has a specific goal your "failure" serves — a security app that "fails" to help burglars — then the complaint profile is different. But for any normal product with a normal user population, ten independent users saying the same thing is a bug report. It deserves the same treatment as a reproducible crash: investigate it, find the cause, fix it.

The inverse is equally true. If one user in a hundred leaves a specific positive comment — *this feature saved me hours* — that 1% saint signal is also disproportionately valuable. They are telling you what is working, which is the Use-Pull signal for what to protect and double down on.

---

## The Def-Push Side: A Different Distribution

The people who build, define, and govern products are not drawn randomly from the same population. The Def-Push role — the architect, the product manager, the standards body member, the platform owner — self-selects for specific traits:

- Strong opinions about how things should be done
- Enough status or position to impose those opinions on others
- Sufficient confidence to define an interface before seeing how it is used

This selection effect produces a Def-Push population with a noticeably different distribution:

- **~50%** — well-intentioned, reasonable, working from their own model of what users need. They are not trying to impose. They have genuinely thought about the problem. They are just working from incomplete information about the Use side.
- **~40%** — well-intentioned but defensive about the Def. Their identity is coupled to the model they built. Use feedback feels like a personal attack. This is the tribal Def problem — not malice, but the structural consequence of having invested your professional identity in a specific answer.
- **~10%** — genuinely listening, updating their model, willing to be wrong. The weak link willing to learn, in the language of this series.

The 40% are not jerks by character. They are people whose Def has become identity. When a user says "this is confusing," the reasonable response is "tell me more." The identity-coupled response is "you are not using it correctly." The difference is not intelligence or goodwill. It is whether the Def is a hypothesis or a declaration.

This is why the Def-Push distortion compounds. The feedback channel already over-represents frustrated users. The Def side already over-represents people who are defensive about their model. The two distortions combine to produce a dynamic where the people most likely to be wrong are the people least likely to update.

---

## Accessibility: The 10x Gutenberg Layer

The 90/9/1 population argument applies directly to accessibility — and produces the most important design insight the series has offered in practical terms.

Accessibility is typically framed as a minority concern. Screen reader users are a small percentage of the population. Keyboard-only navigation affects a fraction of users. High contrast mode is for people with specific visual impairments. The Def-Push response is: design for the majority, add accessibility as an afterthought, treat it as compliance rather than design.

This framing is wrong on its own terms — about 15% of the global population has some form of disability — but it is also wrong about the mechanics. Accessibility is not a feature for a minority. It is a **Gutenberg constraint that forces correct semantic structure for everyone**.

A page with proper heading hierarchy (`h1` → `h2` → `h3`) is navigable by a screen reader. It is also navigable by search engine crawlers, by browser reading modes, by users who use keyboard shortcuts, by users on slow connections who want to skim, and by anyone who returns to the page looking for a specific section. The semantic structure — the logical layer — is independent of the visual presentation. Screen readers reveal whether you actually separated the layers.

A form with proper label associations works for screen reader users. It also works better for autofill, for mobile keyboards that surface the right input type, for users who tab through forms, and for users who make mistakes and need to know which field the error refers to. The correct semantic structure benefits everyone. The wrong structure fails everyone, but fails the screen reader user visibly first.

This is the 10x Gutenberg constraint in action. The screen reader is not an edge case user. It is a strict parser of your semantic layer — it cannot guess, cannot infer from visual proximity, cannot use context the way a sighted user can. When the screen reader fails, it is because your semantic layer was not actually separate from your Gutenberg layer (the visual presentation). The screen reader exposed a coupling that was harming everyone, quietly.

The 10x perspective: at 10x users, the 15% with accessibility needs is no longer a rounding error. It is 1.5x the size of your current user base. Designing them out is not a pragmatic tradeoff. It is YAGNI arrogance — the Def deciding what Use needs without a feedback mechanism to check.

---

## The Long Tail Is 90% of Your Users

The same population math applies to the "power user" trap. Most products are designed by and for the median user as imagined by the Def team. The median user is a fiction — the actual distribution has a long tail of use cases, languages, devices, connection speeds, screen sizes, and contexts that collectively outnumber the imagined median.

The Def-Push response to the long tail is to treat it as edge cases. The Use-Pull reality at 10x scale: the long tail is the majority. The non-English speaker is not an edge case when you have users in 50 countries. The slow connection is not an edge case when you have users in rural areas or on mobile networks. The keyboard-only user is not an edge case when your user base includes people with motor disabilities, power users who prefer keyboard, and people on devices without mice.

At 10x users, the tail wags the dog. The product that was designed for the imagined median user starts failing the actual majority. The silent 90% — who do not complain, they just leave — start leaving in visible numbers.

The 10x moment is when the Def-Push model gets stress-tested by the Use-Pull reality of a population that does not look like the team that built the product.

---

## The Practical Test

Three questions that apply the population argument to any product decision:

**1. If ten independent users say the same thing is broken, is it broken?**
Yes, almost certainly. Investigate it. The phrasing may be wrong. The diagnosis may be wrong. The experience is real.

**2. Does your feedback channel represent your actual users?**
Almost certainly not. The silent 90% are underrepresented. The frustrated 9% are overrepresented. Weight your feedback accordingly — not by dismissing complaints, but by actively seeking the signal from the silent majority: analytics, session recordings, support patterns, churn analysis.

**3. Does your product work without the assumptions built into it by the people who built it?**
Try it without a mouse. Try it with a screen reader. Try it on a slow connection. Try it in a language other than the one your team speaks. Try it as someone who has never used it before and has no idea what your internal model means. Each of these is the grandparent test applied to a specific assumption. The product that passes is the one whose semantic layer is actually independent of the Gutenberg assumptions the Def team baked in.

The user does not sux. The distribution does not lie. Ten independent users saying the same thing is a bug report. Listen to it.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Def-Use, Lean Pull, and Why the User Does Not Sux](https://rinie.github.io/2026/05/16/def-use-lean-pull/) on the feedback loop, and [Competition Is Use-Pull. Monopoly Is Def-Push.](https://rinie.github.io/2026/05/29/competition-use-pull-monopoly-def-push/) on what happens when the feedback loop has nowhere to go.*
