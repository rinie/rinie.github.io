---
layout: post
title: "Penny Recognised Woz: Why the Expert in the Room Misses the Obvious"
date: 2026-07-23
tags: [gutenberg-semantic, def-use, feedback, use-pull, expertise, gemba]
level: general
description: "Steve Wozniak walked into a room full of physicists and they didn't recognise him. The waitress did. Expertise points your attention so hard at one layer that you stop seeing the person standing in front of you. The user who can't parse your jargon is often the one seeing the room clearly."
---

In an episode of *The Big Bang Theory*, Steve Wozniak — co-founder of Apple, the engineer who built the first two Apple computers largely by himself — appears as himself in a restaurant. The four scientists at the table, men who can derive quantum field theory on a napkin, do not recognise him. Penny, the waitress with no technical background whatsoever, recognises him instantly and is thrilled.

That scene is the whole argument of this post, and it is funnier than the argument, so keep the scene in mind while I make the argument.

---

## Expertise Points Your Attention

The physicists missed Woz because their attention was pointed, hard, at their own layer. They were deep in their domain — the conversation they were having, the problems they care about, the credentials that signal who matters in their world. Wozniak doesn't register on those instruments. He's not a physicist. He doesn't publish in their journals. The filter that tells them who is worth noticing was tuned for a different room.

Penny had no such filter. She looked at the person in front of her and saw someone she recognised, because she was looking at the person rather than at the domain. The thing the experts were too expert to see was sitting at their own table.

This is a specific, recurring failure, and it isn't about intelligence. The physicists are not dumb. They are *miscalibrated* — their attention is so well-tuned to one layer that signals from outside that layer don't land. The series has a name for the layer split: the Gutenberg layer (the infrastructure, the implementation, the domain machinery) underneath the Semantic layer (what it means, what it's for, who it serves). The experts live below the waterline, in the machinery. Penny lives above it, where the actual person is.

---

## The User Who Can't Parse Your Jargon Is Often Seeing Clearly

Take that scene out of the restaurant and put it in any software team.

The engineer who built the system knows every internal detail. They know why the button is where it is, what the error code means, which step has to happen first. Their attention is pointed at the machinery. When a new user fumbles — can't find the button, doesn't understand the error, does the steps in the wrong order — the engineer's instinct is that the *user* missed something.

Usually it's the reverse. The user, with no investment in how the machinery works, is looking at the thing the way an ordinary person looks at it — which is exactly the view the engineer has lost the ability to see. The user who says "I have no idea what this screen wants from me" is not failing to recognise Woz. They are Penny, telling you something true about the room that you are too expert to perceive.

This is why watching a real person use your system is worth more than any amount of internal review. Not because the user knows more — they know far less about the machinery. Because they can still see the layer you stopped seeing the day you learned how it worked underneath.

---

## Penny and the Flat-Pack Closet

Another scene, same character, sharper point. Penny needs to assemble a flat-pack closet. The guys offer to help, and immediately begin analysing the design — the joints are suboptimal, the engineering could be reinforced, here is the correct methodology. They are not wrong about the engineering. They are completely wrong about the task. The task was never "evaluate the closet." The task was "have a closet."

Penny assembles it herself, with the wrong word for the tool and the right grip on the problem. She looks at the parts in front of her, finds the piece that turns the screws, and turns them. The closet stands up. The clothes have somewhere to go.

The guys' analysis is what panic looks like when smart people do it. When you don't know where to start, calculating *feels* like progress — you're producing something, it's rigorous, it has the texture of work. But the closet isn't getting built while you do it. Penny didn't panic. She went to where the work actually was — the physical parts, telling her what to do next — and started. That's the whole discipline: when the instructions fail you, look at the actual object instead of the diagram of the object.

---

## Looking at the Person, Not the Domain

The fix, in both scenes, is the same and it's small. Look at the layer you stopped seeing.

Penny recognised Woz because she looked at the person and not the credentials. She built the closet because she looked at the parts and not the methodology. In both cases the experts had more knowledge and saw less, because their knowledge pointed their attention away from the thing that actually mattered — the person at the table, the closet on the floor.

The user who can't read your interface is doing you the same favour Penny did the physicists. They're recognising what you've become too expert to see. The only mistake available to you is the physicists' mistake: being so sure the important signal comes from your own layer that you miss the obvious thing sitting right in front of you.

The expert isn't wrong about the machinery. They're just not the one who can see the room anymore. Someone has to be Penny. If it isn't you, find the person who is, and watch what they notice.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Def-Use, Lean Pull, and Why the User Does Not Sux](https://rinie.github.io/2026/05/16/def-use-lean-pull/) on the user as the signal you can't generate internally, [Going to the Gemba: Getting Your Feet Wet at the Waterline](https://rinie.github.io/2026/06/17/going-to-the-gemba/) on watching the actual work instead of the diagram of it, and [Nothing Is Confusing to Me: The Inmates Are Running the Asylum](https://rinie.github.io/2026/06/20/nothing-is-confusing-to-me/) on proximity blindness in your own domain.*
