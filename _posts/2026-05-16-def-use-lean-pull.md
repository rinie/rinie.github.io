---
layout: post
title: "Def-Use, Lean Pull, and Why the User Does Not Sux"
date: 2026-05-16
---

The [Gutenberg/Semantic model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) describes how physical and logical layers relate in software and information systems. That framework is mostly about the *production* side — how artifacts are made, versioned, and moved. This post adds the *consumption* side, which is a different axis entirely.

---

## 1. The Def-Use Split

In compiler theory, **Def-Use** describes where a variable is defined versus where it is used. They have different lifecycles, different owners, and the relationship between them is not always what the author intended.

The same split exists in every semantic layer:

- **Def** — the author's intent: the chapter structure, the API contract, the schema, the component design, the feature set. The author has a semantic model of what they made and what it means.
- **Use** — the reader's interpretation: what they actually do with it, what meaning they construct, which path they take, which features they ignore.

These are **not the same semantic layer**. They merely overlap. The gap between them is where most of the interesting things happen — and most of the waste.

A chapter titled "Advanced Configuration" means something different to an expert than to a beginner. A REST API designed around resources gets used as an RPC system. A book index built by the author may not contain the terms a reader would actually search for. A feature built by a product team sits unused because users solved the problem a different way. The Def is a proposal. The Use is the verdict.

---

## 2. Gutenberg Was Pure Def Push

The original Gutenberg press was a **pure Def-push system**. The author writes, the press prints, the reader reads what was printed. There is no feedback loop. The semantic model flows in one direction — from author to reader — and the reader has no channel to push back.

This was an improvement over manuscript culture (one scribe, one reader, bespoke) but it locked in the author's semantic model as the only one that mattered. The index and the table of contents are the author's semantic map of their own content — not the reader's. A reader who approaches a book differently from how the author intended it has no recourse except to stop reading.

Software inherited this model. Waterfall development is Gutenberg-push: requirements defined, system built, system delivered. The user gets what the author decided they needed. The Def is the specification; the Use is assumed to match.

---

## 3. The Train-ing Problem

When the Def is so rigid that the Use has no room to diverge, you are no longer driving — you are being taken for a ride.

A car gives you a steering wheel. You decide where to go. The road is the Gutenberg layer (physical constraint), but the route is yours (semantic choice). A train gives you a seat. The track was laid by someone else, the schedule was set by someone else, and your only choice is whether to board. The semantic model of "where to go and when" has been entirely captured by the Def side. The Use side has been eliminated.

**Train-ing** — the pattern of designing systems that put users on rails — appears everywhere:

- Software wizards that enforce a fixed sequence of steps regardless of what the user actually needs
- APIs that only support the use cases the author anticipated, with no escape hatch
- Documentation structured around how the system was built rather than what the user is trying to do
- Onboarding flows that teach the author's mental model rather than discovering the user's
- Features that require the user to adopt the author's terminology before they can do anything

In each case the Def has colonised the Use. The user is not driving. They are passengers on the author's track.

The irony, as with all Gutenberg/Semantic boundary violations, is that this is presented as a feature — "opinionated software", "guided experience", "best practice workflow". Sometimes it is. More often it is the author refusing to listen.

---

## 4. Journalists, Front Pages, and the Missing Editor

Strong object-oriented programming has the same problem as a newspaper where every journalist writes their own headline and all of them want to be on the front page.

Each journalist owns their domain completely. Their story is well-researched, their prose is polished, their headline is clever. In OOP terms: each class is perfectly encapsulated, each interface is carefully designed, each abstraction is internally consistent. The Def side is impeccable.

But twenty journalists each sovereign over their own story, each pushing their own Def as the primary concern, produces a newspaper nobody can read. There is no coherent front page. There is no sense of what matters most today. There is no reader's perspective anywhere in the production process. The semantic models of twenty authors collide without resolution.

This is Spring/Java enterprise architecture. It is also what happens when the Gang of Four patterns are applied without restraint — impressively structured, practically incomprehensible to anyone who was not in the room when it was built.

**The editor is the Use-side voice inside the Def process.** Not a gatekeeper deciding what the user is allowed to receive — that is train-ing with a press badge. The editor asks: what does the reader actually need today? Which story deserves the front page? How much space does this warrant? Which headline will be *understood* rather than admired by its author?

The editor is a proxy for the reader's semantic model while the content is still being shaped. They hold the Def accountable to the Use before the ink dries. In software terms this is the architect, the tech lead, the API reviewer, the UX researcher — whoever is in the room asking "but what does the user actually do with this?" before the interface is shipped.

The failure mode of strong OOP is the absence of this role. Encapsulation is a journalist's right. It protects the author's domain. But a newspaper run entirely by journalists with no editorial layer produces content that is technically correct and editorially incoherent — each piece perfectly formed, the whole unreadable.

The editor does not make journalists worse. They make the newspaper better by ensuring the Def serves the Use rather than the author's self-image.

---

## 5. Lean Pull: Value Is Defined by the User

Lean manufacturing's core principle is that **value is defined by the customer, not the producer**. Pull means work is triggered by demand, not pushed by supply. You do not make things and hope someone wants them; you discover what is wanted and make exactly that.

Applied to the Def-Use split:

- **Push (Def-driven)**: the author defines the semantic model, builds to it, delivers it, and expects the user to adapt. Features ship because they were in the spec. Documentation is written because the system was built. The author's semantic model is the ground truth.
- **Pull (Use-driven)**: the user's actual behaviour — what they search for, where they get stuck, what they work around, what they abandon — defines what gets built next. The Use is the ground truth. The Def is a hypothesis to be tested.

Waste in the Lean sense is anything produced that does not correspond to actual Use. A feature nobody uses is waste. Documentation nobody reads is waste. An API endpoint never called is waste. The gap between Def and Use is the waste inventory — work that was pushed without being pulled.

---

## 6. I Am a Weak Link Willing to Learn

The hardest part of closing the Def-Use gap is the author's ego.

Defining things feels like expertise. The author knows the system. They designed it. They understand it deeply. When a user does not use it as intended, the tempting conclusion is that the user is wrong — that they need more training, better documentation, a longer onboarding sequence. Put them on the train. Teach them the track.

The alternative is to recognise that **the author is the weak link**. The user's confusion is not a failure of the user to understand the author's semantic model. It is a failure of the author's semantic model to match the user's reality. The user is always right about their own experience. They may be wrong about the solution, but they are never wrong about the problem.

This is not a comfortable position for an author to take. It requires treating every piece of negative feedback not as noise to be filtered but as a signal to be decoded. It requires holding the Def lightly — as a proposal, not a declaration. It requires being willing to restructure the semantic model around the Use rather than defending the Def.

**Willing to learn** is the operational posture: ship a hypothesis, observe the Use, update the Def, repeat. The feedback loop is the product. The semantic model is never finished.

---

## 7. The User Does Not Sux

UX — User Experience — is the discipline that exists to close the Def-Use gap. The name is telling: it centres the **Use**, not the **Def**. Not "author experience" or "system experience" — the experience that matters is the one the user actually has.

**The user does not sux** has a double meaning:

- The user does not *suck* — they are not the problem. The gap between Def and Use is the problem, and closing it is the author's job, not the user's.
- UX does not suck — it is not a cosmetic layer painted over a broken Def. Done properly, UX is the discipline of discovering the user's actual semantic model and restructuring the Def to match it.

The failure mode is treating UX as a Def-push operation: the author decides what the experience should be, designs it, and delivers it. This is train-ing with better graphics. Real UX is a Use-pull operation: observe behaviour, discover the gap, redesign the Def. The user is the source of truth, not the audience for a presentation.

---

## 8. Feedback Loops in the Gutenberg Framework

The original Gutenberg model was unidirectional. Every layer in the stack — Gutenberg 1.0, 2.0, 2.1 — describes how the Def moves from author to user. But Gutenberg 2.1 also introduced the infrastructure for feedback:

- **git issues** — users signal where the Def does not match their Use
- **npm download counts** — the market reveals which Defs are actually being pulled
- **Search queries** — the user's own words, not the author's terminology
- **Error logs and stack traces** — the Use layer reporting failures in the Def layer
- **A/B testing** — running two Defs simultaneously to let Use select the better one
- **Stack Overflow** — a collective index built from the Use side, not the Def side

These are all mechanisms for **Use to feed back into Def**. The web made this bidirectional in a way the printing press never could. The author who ignores these signals is choosing to stay on the push side — defining in isolation, shipping on a schedule, measuring success by delivery rather than adoption.

The Lean pull principle says: the feedback loop is not optional. It is the product. The Def that does not listen to Use eventually becomes irrelevant — not because it was wrong, but because it stopped learning.

---

## 9. Moving Fast Without Breaking Things — the Use Edition

The original Gutenberg post argued that clean Gutenberg/Semantic boundaries let you move fast without breaking things on the *technical* side — evergreen runtimes, semver, portable containers.

The Def-Use split adds the *human* side of the same principle:

- A stable, narrow Def interface — a well-designed API, a clear mental model, a consistent vocabulary — lets users build on top of it without being broken by changes underneath
- A Use-informed Def — one that was shaped by actual feedback — is more stable than one designed in isolation, because it aligns with what users actually need rather than what the author assumed they needed
- The author who treats their Def as provisional and listens to Use is doing semantic versioning on their own ideas: shipping hypotheses, observing the verdict, incrementing the model

**Move fast without breaking the user.**

That is the Use-pull version of the principle. Not just: do not break the build. Do not break the person trying to accomplish something. Keep the Def close enough to the Use that the gap never becomes a wall.
