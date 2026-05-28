---
layout: post
title: "Def-Use, Lean Pull, and Why the User Does Not Sux"
date: 2026-05-16
category: "Framework"
tags: [framework, ux, feedback-loops, use-pull]
depth: conceptual
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

The Def is also **tribal and inside**. The Java community, the SAP ecosystem, the TOGAF-certified architect — these are tribes whose identity is bound up in the Def. The Def is not just a technical choice; it is a social one. Challenging the Def is not just a technical disagreement, it is a threat to the tribe's identity. This is why feedback that contradicts the model gets reframed as user error rather than model failure — not out of malice, but because the tribe genuinely cannot separate the two. The model *is* the tribe. Defending the Def is defending the group.

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

Marissa Mayer's infamous test of 41 shades of blue at Google is the most quoted example of A/B testing as Use-pull — and the most instructive about its limits. Google measured click-through rates on 41 slightly different shades of hyperlink blue and picked the winner. Estimated revenue impact: $200M/year. Designers were furious.

Both sides were right about different things. Mayer was right that designer intuition should not be immune to Use feedback — "I know what looks right" is Def-push, and the $200M was real. Designers were right that click-through rate is a thin proxy for the Semantic value you are actually trying to create. Design coherence, brand identity, and aesthetic consistency are real things that a single A/B test cannot measure. You can optimise a local maximum and destroy the global one.

The 41 shades of blue is what happens when you have a Use-pull instrument but no Semantic theory of what you are optimising for. A/B testing measures clicks — a Gutenberg signal (did the byte get transmitted?) — not satisfaction, trust, or long-term relationship — the Semantic question you actually care about. Use-pull without a Semantic hypothesis is just noise optimisation. The weak link willing to learn listens to Use signals and interprets them — they do not simply execute whatever the data says.

This is the **vanity metric** problem, named by Eric Ries in The Lean Startup. A vanity metric is measurable, attributable, and optimisable — it generates a number with a dollar sign next to it — but it does not tell you whether to change the Def or keep it. Click-through rate looks like a Use signal. It feels data-driven. But it measures *did the user click* not *did the user get what they needed*. It is a Gutenberg signal (the byte was transmitted) mistaken for a Semantic signal (the user found what they were looking for).

The 41 shades story has all the vanity metric hallmarks. It optimises a proxy — clicks correlate with revenue but don't measure search satisfaction, trust, or long-term retention. It has no falsifiable hypothesis — there was no theory of *why* a particular blue would perform better, just measurement of which one did. You cannot learn from that; you can only repeat the experiment. It risks a local maximum trap — A/B testing your way to a colour that wins every pairwise comparison but degrades brand coherence in ways the click metric cannot see.

The contrast is **actionable metrics** — ones tied to a specific hypothesis about user behaviour that can tell you whether the Def needs to change:
- Time to first successful result
- Return visit rate
- Query reformulation rate — how often users rephrase, which is a signal of failure
- Task completion rate

These are harder to measure and don't generate a clean headline number. But they are closer to the actual Semantic question: did the user accomplish what they came to do?

Vanity metrics are also **Def-push in disguise**. They look like Use-pull — you are measuring user behaviour — but they are actually the author choosing which behaviours to measure and declaring those the ones that count. Mayer decided clicks were the signal. That is still a Def. Just one with a spreadsheet attached.

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

---

## 10. Models, Architects, and You're Holding It Wrong

Enterprise Architects are the ultimate journalists who all want the front page. They define the canonical data model, the integration patterns, the reference architecture — all Def, no Use. By the time the model reaches a user it has passed through so many editorial layers, all of them on the Def side, that the original problem the user had is unrecognisable. The architecture *is* the product. The user is an implementation detail.

**Models** — data models, domain models, UML diagrams — have the same problem. They reify the author's semantic understanding into a formal artifact that then becomes load-bearing. Once the model is the ground truth, feedback that contradicts it becomes a data quality problem or a user education problem, never a model problem. The model is defended rather than updated. The Def has become a wall.

This is why "you're holding it wrong" is the purest possible expression of Def-push failure. Steve Jobs at antenna-gate had a semantic model so complete in his own mind that the user's physical reality — the hand, the antenna, the dropped signal — became the user's fault. The Def was correct. The Use was wrong. This is the logical endpoint of ignoring feedback: the author's model becomes unfalsifiable.

Jobs is the interesting edge case because he also produced things users hadn't known they wanted. The distinction matters:

- **Use-pull Jobs**: showing users a possibility they hadn't imagined, then watching how they actually used it and adapting — still Use-pull, just with a longer feedback loop and stronger authorial instinct
- **Def-push Jobs**: deciding the Def is complete and the Use must conform — antenna-gate, removing the headphone jack without an adapter, no ports on the MacBook

The difference is whether you listen *after* you ship. Strong intuition can shorten the feedback loop. It cannot replace it.

---

## 11. Build-Measure-Learn versus Plan-Do-Check-Act

The Lean Startup's **Build-Measure-Learn** loop and the PDCA cycle (**Plan-Do-Check-Act**, also called the Deming wheel) look similar on the surface. Both are iterative. Both involve doing something and then looking at what happened. But they point in opposite directions.

**Plan-Do-Check-Act** starts on the Def side:
- **Plan** — define what you intend to do (Def)
- **Do** — execute the plan (push)
- **Check** — verify the result against the plan (did the Use match the Def?)
- **Act** — correct deviations from the plan (bring Use back to Def)

The feedback loop in PDCA is about **conformance**. Did reality match the plan? If not, fix reality. The Def is the standard. The Use is the variable to be controlled. This is appropriate for manufacturing quality control — you want the ten-thousandth widget to match the specification as closely as the first. The Def is correct and the Use (the production process) must conform.

**Build-Measure-Learn** starts on the Use side:
- **Build** — ship the smallest possible Def hypothesis (MVP)
- **Measure** — observe actual Use behaviour, not conformance to plan
- **Learn** — update the Def based on what Use revealed

The feedback loop in Build-Measure-Learn is about **discovery**. Did the Def match what users actually needed? If not, fix the Def. The Use is the standard. The Def is the variable to be updated. This is appropriate for product development — you do not know in advance what the user needs, so you make the feedback loop as short as possible and treat every iteration as a question, not a delivery.

The confusion between the two loops is a major source of dysfunction in software organisations. Agile sprints are often run as PDCA — plan the sprint (Def), execute the sprint (push), check the burndown (conformance), act on deviations (bring Use back to Def). The retrospective asks "did we do what we said we would do?" rather than "did what we did actually help the user?" The loop closes on the Def side, never on the Use side.

Build-Measure-Learn insists the loop closes on the Use side every time. The sprint review is not a demonstration of conformance to the plan. It is a measurement of whether the Def moved closer to what users actually need. The plan is always provisional. The user is always right about their own experience.

The weak link willing to learn chooses Build-Measure-Learn not because PDCA is wrong but because PDCA assumes the Def is already correct. In product development, that assumption is almost never warranted at the start — and the cost of defending a wrong Def grows with every iteration that ignores the Use signal.

Stewart Brand's pace layering describes this accurately at a civilisational scale — fashion changes faster than governance, governance faster than infrastructure. Brand himself was Use-pull in spirit: the Whole Earth Catalog was tools for people to do things themselves, evaluated by users, not curated by experts. But Gartner took pace layering and turned it into a justification for the Def-side hierarchy — "infrastructure is slow to change therefore architects should control it." That is a non-sequitur dressed as strategy. The fact that a layer changes slowly does not mean it should be controlled by a tribe whose identity depends on it not changing. It means the feedback loop is longer, not that it should be closed off.

---

## 12. Gutenberg Started as Push — But 10x Readers Made Pull Inevitable

Gutenberg 1.0 was pure Def-push by necessity. The bottleneck was production — copying manuscripts was so expensive that anything printed was automatically worth reading. The author's semantic model was the only one that could exist because there was no economic feedback loop. The reader was grateful for whatever arrived.

But 10x readers changed the economics completely.

**Scale created competition** — multiple authors, multiple publishers, multiple booksellers competing for the same reader's attention. The reader gained power simply by having choice. The Def was no longer automatically correct just because it was the only one available.

**Editors emerged** as the first institutional Use-proxy — someone inside the Def process whose job was to ask "will the reader understand this?" before ink met paper. Not a gatekeeper. A proxy for the reader's semantic model inside the production process.

**Publishers** became the first market signal — they would only invest in printing what they believed readers would buy. The first crude Build-Measure-Learn loop: print a run, measure sales, learn what to print next. The Def became provisional.

**Newspapers** compressed the feedback loop to daily cycles. A story nobody read was gone tomorrow. The front page was decided by what sold papers, not what the journalist wanted to write. The Use signal became the editorial standard.

**Literacy itself** grew in response to Use — as more people read, the Def had to adapt to more diverse semantic models. Simpler sentences, clearer headlines, shorter paragraphs. The reader shaped the writing, not just the other way around.

So Gutenberg 1.0 started as pure push but **the infrastructure it created made pull inevitable**. More copies meant more readers. More readers meant more feedback. More feedback meant editors, publishers, and eventually market research. The printing press was a Def-push technology that accidentally bootstrapped the Use-pull economy.

The same arc repeats at every scale jump:

| Scale jump | Push phase | Pull emerges |
|---|---|---|
| Gutenberg press | Author pushes to grateful readers | Publishers, editors, newspaper market |
| Broadcast TV | Networks push to passive viewers | Ratings, focus groups, cable choice |
| Early web | Webmasters push HTML pages | Search engines, analytics, A/B testing |
| App stores | Developers push to early adopters | Reviews, uninstall rates, retention metrics |
| LLMs | Models push to early users | RLHF, fine-tuning, user feedback loops |

Every time a new medium scales up, it starts as push and develops pull mechanisms as the reader-to-author ratio grows. The Use signal becomes too loud to ignore.

The 10x readers insight is that **pull is not a design choice — it is what happens when Def-push scales**. You can resist it, like Steve Jobs at antenna-gate, but the market eventually enforces the Use perspective whether the author wants it or not. The weak link willing to learn does not wait for the market to force the lesson. They build the feedback loop in from the start.

---

## 13. GNU versus Linux: The First Internet-Visible Community Def-Use Loop

**GNU was pure Def-push.** Richard Stallman had a complete, philosophically coherent semantic model: free software, copyleft, the GNU system as a unified whole. He defined the tools (gcc, glibc, bash, make), the license (GPL), the ideology (software freedom as a moral imperative), and the architecture. The Def was meticulous and principled. He knew exactly what the free software world should look like and built toward it for a decade.

But GNU never shipped a usable kernel. The Hurd — GNU's kernel — was architecturally ambitious (a microkernel, because that was the *correct* design) and essentially unusable in practice. The Def was so complete and so defended that Use feedback could not get in. The kernel had to be right before it could be released. It never was.

**Linux was Use-pull from the first post.** Linus's 1991 announcement was famously self-deprecating: *"just a hobby, won't be big and professional like gnu."* He shipped something that worked on his hardware, asked what other people wanted, and iterated on feedback. The first kernel was crude, monolithic (architecturally wrong by Stallman's and Tanenbaum's standards), and tied to specific hardware. It did not match any canonical Def. It matched what users could actually run.

The Tanenbaum-Torvalds debate in 1992 is the Def-Use split made explicit in public:

- **Tanenbaum (Def)**: monolithic kernels are obsolete, microkernels are architecturally correct, Linux is a step backward
- **Torvalds (Use)**: it runs, people use it, it gets better from feedback, architectural purity is less important than working software

Tanenbaum was right about the architecture. Torvalds was right about the feedback loop. Minix never became an OS anyone ran. Linux runs everything.

**GNU without Linux** is the enterprise architect's reference architecture — complete, correct, principled, and waiting for users to conform to it. **Linux without GNU** is the MVP — incomplete, rough, immediately useful, shaped by whoever showed up and filed a bug. The irony is that GNU tools run on Linux everywhere. The Def side provided essential infrastructure. But the Use side provided the feedback loop that made it matter. Neither alone was sufficient — the Def is not worthless, it just cannot be the only voice in the room.

### Linux as the first internet-visible community development

Linux was also something new: **the first large software project developed visibly on the internet**, with the Use signal built directly into the development process. The mailing list was not a support channel bolted onto a finished product. It was the feedback loop. Bug reports, patches, flame wars, hardware requests — all of it shaped the Def in near-real-time. The community *was* the editor.

Clay Shirky's *Here Comes Everybody* (2008) named what Linux had already demonstrated: the internet collapsed the institutional cost of collective action. You no longer need an organisation — a Def-side institution — to coordinate group behaviour. The Use side can organise itself. Wikipedia, open source, Stack Overflow — these are Use-side communities producing Def-side artifacts without a central author. Shirky's concept of **cognitive surplus** makes the scale explicit: the collective spare capacity of internet users dwarfs anything a Def-side institution can deploy. The tribe is always smaller than the community. GNU was a tribe. Linux became a community.

This pattern then repeated at every layer of the stack:

**VS Code versus Eclipse** — VS Code's extension API and issue tracker are open. Users file issues, vote on features, submit PRs. The Def evolves in response to visible Use signals. Eclipse's development was largely internal to IBM and the Eclipse Foundation — a smaller, slower, more Def-push loop. The community could observe but not easily steer.

**Markdown versus HTML** — HTML was designed by committee (W3C), with a formal specification process, versioned releases, and a deliberate Def-push structure. Markdown was created by John Gruber in 2004 as a practical tool for writers, iterated rapidly on feedback, and spread because it matched how people actually wanted to write. It has no formal committee. Its Def is shaped entirely by Use — what parsers implement, what platforms support, what writers adopt. CommonMark emerged later as a community-driven attempt to formalise what Use had already decided.

**TypeScript versus Java** — Java's type system was designed upfront, versioned slowly, and governed by the JCP (Java Community Process) — a formal Def-push institution. TypeScript ships continuously, responds to community feedback on GitHub, and has explicitly said its goal is to follow JavaScript rather than lead it. The type system is a Def that serves the Use (existing JavaScript codebases and patterns) rather than demanding the Use conform to the Def. Anders Hejlsberg has described TypeScript's design process as listening to what developers actually do and making that safe — the weak link willing to learn, at language design scale.

The common thread: **the tools that opened their Def to internet-visible community feedback won**. Not because community feedback is always right, but because the feedback loop compressed the distance between Def and Use fast enough to matter. The tools that defended their Def from inside a committee or a corporation arrived at the right answer too slowly and too late.

---

## 14. AI Models and the Same Pattern

The Def-Use split applies to AI assistants as directly as it applies to IDEs, browsers, and kernels.

The "magical black box" model is Def-push by design. The model has a complete answer, delivers it confidently, and the interaction is one-directional. The user receives the output. This is the deep research report, the comprehensive single-shot response, the confident document that arrives fully formed. Ask, receive, done. The model's Def wins by default because the interaction is too fast and too complete to leave room for Use feedback. You are not driving. You are being taken for a ride — an impressive, fluent, well-researched ride, but a ride nonetheless.

The steerable model holds its Def more lightly. It takes smaller steps, checks in, leaves room for the user to redirect. The interaction is collaborative rather than transactional. The user's semantic model gets heard and shapes the output. The Def is a proposal at each step, not a declaration at the end.

The risk of the magical black box is that the user's actual goal — what they were really trying to do — never surfaces. The model's Def colonises the Use. The risk of the steerable model is the opposite: too much checking in, not enough confident Def to push against. The user needs something to react to. A model that never commits is as unhelpful as one that never listens.

The sweet spot is what good editors do: bring a strong enough Def to be useful, hold it lightly enough to be correctable. Confident enough to push back. Humble enough to update. The weak link willing to learn — at model scale.

The 41 shades of blue problem applies here too. Optimising on thumbs-up ratings is a Gutenberg signal — did the user click approve? Whether the user actually got what they needed is the harder Semantic question. All AI labs are figuring out how to close that loop. None of them has solved it. The Def-Use gap in AI is the same gap it has always been — the author's model versus the user's reality — just running faster and at greater scale.
