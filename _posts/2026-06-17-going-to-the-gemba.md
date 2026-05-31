---
layout: post
title: "Going to the Gemba: Getting Your Feet Wet at the Waterline"
date: 2026-06-17
tags: [gutenberg-semantic, waterline, gemba, lean, use-pull, feedback]
level: general
description: "The Gemba is the actual place where work happens. The sticky note next to the monitor is a bug report nobody filed. The workaround in the onboarding documentation is a design failure nobody admitted. Go to where the Use signal is generated. Watch without helping. Get your feet wet at the waterline."
---

The architecture diagram shows where the Gutenberg and Semantic layers were supposed to meet. The Gemba shows where they actually meet.

Gemba (現場) is a Japanese word meaning "the actual place." In lean manufacturing it means go to where the work happens — not the meeting room, not the dashboard, not the manager's summary of what the workers are doing. The floor. The machine. The moment the product is made or the service delivered.

The Gemba walk is Use-Pull made physical. You go where the Use signal is generated, before it has been filtered, summarised, averaged, and presented back to you in a form that is comfortable to receive. You watch. You do not help. You notice what is actually happening rather than what you designed to happen.

In software the Gemba is the moment a user encounters your system. The SQL query running at 3am. The form field confusing the new hire on their first day. The error message that sends the support ticket. The dropdown that nobody clicks because nobody knows what it does.

Go there. Watch. Get your feet wet.

---

## The Sticky Note Is a Bug Report

There is a specific artifact that appears in every workplace where the waterline is muddy: the sticky note next to the monitor.

*"When you see ERROR_4721 press F5 and try again."*
*"Don't use the Export button — use Reports > Legacy > Export instead."*
*"Call Henk if the sync fails, it happens every Tuesday."*

Each sticky note is a post-it confession. The system said one thing. The user needed another. The sticky note is the resolver they built themselves because the system did not provide one. It is a bug report written in yellow paper and stuck where the evidence is most visible — right next to the thing that keeps failing.

The sticky note is also invisible to the Def. It never entered the bug tracker. It was never in the sprint backlog. It does not appear in the architecture diagram. The Def does not know it exists because the sticky note is the Use signal that gave up trying to reach the Def and solved the problem locally instead.

The Gemba walk surfaces these. You walk to the desk, you look at the monitor, and you read the waterline's autobiography in yellow paper and blue ink.

---

## Watch Without Helping

The critical discipline of the Gemba walk is to observe without intervening. The moment you help, you hide the friction.

*"Oh, just click here"* — removes the signal that the button was in the wrong place.
*"That's the legacy path, we don't use that anymore"* — removes the signal that the old path is still being used because the new one is harder to find.
*"You just need to remember to do X first"* — removes the signal that X should happen automatically.

Each intervention is kindness that costs information. The user is grateful. The design flaw is buried. The waterline stays muddy.

Watch what people do that is not in the documentation. Watch what they avoid. Watch where they hesitate. Watch where they sigh. Watch the keyboard shortcut they discovered and never told anyone about because it was not in the manual. Watch the window they always keep open in the background because closing it means a two-minute wait to reopen it.

The hesitation is the muddy waterline. The sigh is the Use signal. The undocumented shortcut is the user building their own resolver because yours was too slow.

---

## The Workaround Taxonomy

Workarounds are the most honest Use signal available — more honest than surveys, more honest than support tickets, more honest than user research sessions where people tell you what they think you want to hear. A workaround is what people actually do when the system fails them. It is revealed preference at its most unambiguous.

**Type 1 — The sticky note.** Local, personal, the user solved it for themselves. Invisible to the system. Invisible to the Def. Lost when the user leaves or the monitor is replaced. Freshest signal: this is a problem someone encountered recently enough to write it down.

**Type 2 — The tribal knowledge.** *"Ask Sarah, she knows how to do it."* The resolver is a person, not the system. The workaround lives in Sarah's head. When Sarah leaves, the workaround breaks and everyone discovers simultaneously that they did not know how to do it. The Gemba walk finds this in the sentence "I'm not sure, I usually ask Sarah."

**Type 3 — The shadow system.** The Excel spreadsheet that tracks what the official system should track. The Access database the team built because the official database could not do what they needed. The shadow system is the Use signal that gave up waiting for the Def to respond and built its own resolver. It is the most expensive workaround and the clearest possible signal that the waterline is not just muddy but in the wrong place entirely.

**Type 4 — The normalised dysfunction.** The workaround so embedded in daily practice that nobody mentions it during onboarding because it is simply "how things work." The DBA who has been maintaining query hints for three years without telling anyone because "that's just what you do with this database." The support team whose first response to every error is a known sequence of steps that fixes it without anyone knowing why. The normalised dysfunction is the oldest Use signal — so old that it has stopped being perceived as a signal at all.

Each type represents a different distance between the Use signal and the Def. Type 1 is fresh. Type 4 is archaeology. The Gemba walk surfaces all of them — but you have to be looking.

---

## SQL at the Gemba

The DBA who goes to the Gemba of their database reads a different kind of sticky note.

Query hints are the database's sticky notes: `/*+ INDEX(sales idx_sales_region) */` written by someone who noticed the optimiser was making the wrong choice and fixed it locally without changing the query the application sends. The application thinks it is sending a clean semantic request. The DBA has been quietly correcting the Gutenberg execution for years.

Indexes that exist for reasons nobody remembers are the tribal knowledge. The index was created for a query that no longer runs, or for a report that was decommissioned in 2019, but nobody dropped it because dropping indexes is scary and everyone vaguely remembers that "it was there for a reason."

Comments that say `-- DO NOT CHANGE — no idea why this works but it does` are the normalised dysfunction at its most honest. Someone fixed a bug. They do not know why the fix works. They documented their uncertainty in a comment and moved on. The comment has been in production for seven years.

The Gemba walk of a legacy database is reading these artifacts and reconstructing the history of every moment the Gutenberg layer moved and the Semantic layer had to adapt informally rather than formally. Each one is a moment the waterline shifted and someone fixed it with a sticky note in SQL rather than by cleaning the waterline properly.

---

## Going to the Right Gemba

The Gemba walk is only useful if you go to the right Gemba — the place where the Use signal you most need to hear is being generated, not the place where the users are already happy.

Steve Jobs went to the Gemba selectively. Apple Store customers reacting to new products. Focus groups for launches he cared about. Not the developer writing App Store apps at 30% commission. Not the enterprise IT administrator managing locked-down MacBooks. Not the user standing outside trying to make a call while holding their phone in a way that broke the antenna. The Gemba walk works when you go to the friction, not the applause.

The Gemba walk fails when you go to the users who have already adapted — who have internalised the workaround, who no longer notice the sticky notes because they have been there so long. They will tell you the system works fine. It does work fine, for them, now, after three years of adaptation. The new user, the occasional user, the user trying to do something slightly different — they are the Gemba that matters, and they are the hardest to find.

---

## Getting Your Feet Wet

The waterline is not visible from the architecture diagram. It is visible from the Gemba — the actual place where the Gutenberg layer and the Semantic layer meet in practice, in the hands of the people who use the system every day.

The sticky note next to the monitor. The workaround in the onboarding documentation. The shadow Excel spreadsheet. The query hint that has been there for three years. The `-- DO NOT CHANGE` comment. The call to Henk every Tuesday.

Each one is the waterline made visible. Each one is the Use signal that could not reach the Def through the normal channels so it routed around them. Each one is the system telling you, in its own way, what it could not tell you in a meeting.

Go to the Gemba. Watch without helping. Read the sticky notes. Get your feet wet at the waterline. That is where the work actually is.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Hiding the Waterline Makes You Drown Without Knowing Why](https://rinie.github.io/2026/06/08/hiding-the-waterline/) on making the boundary visible, [Revisiting the Waterline: Small Fixes, Five Years Later](https://rinie.github.io/2026/06/10/revisiting-the-waterline/) on what to do after the Gemba walk, and [Def-Use, Lean Pull, and Why the User Does Not Sux](https://rinie.github.io/2026/05/16/def-use-lean-pull/) on the feedback loop the Gemba walk reopens.*
