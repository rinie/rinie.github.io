---
layout: post
title: "Do You Trust This Document? No, I Have to Read It First."
date: 2026-06-23
tags: [gutenberg-semantic, wasm, security, trust, sandbox, boundary]
level: technical
description: "The WASM sandbox puts the security boundary in the Gutenberg layer where it can be enforced structurally. The 'do not click external links' policy puts it in the Semantic layer — the user's judgment — where it degrades under pressure and fails predictably at scale. One is engineering. One is hope."
---

There are two ways to prevent untrusted code from doing something it should not.

The first: make it structurally impossible. The runtime defines exactly what the code can access. Memory is bounded. System calls are mediated. Network access requires explicit permission. The code literally cannot escape the boundary by design — not by instruction, not by policy, not by asking nicely. This is the WASM sandbox.

The second: ask the user to be careful. Send a warning. Write a policy. Train the staff. Remind people that external links may be dangerous. Hope they remember when it matters. This is "do not click on external links."

One of these is engineering. One is hope.

---

## The Gutenberg Boundary and the Semantic Instruction

The [Gutenberg/Semantic model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) describes every system as having a physical layer and a logical layer. Security boundaries work the same way.

A **Gutenberg security boundary** is enforced at the physical execution layer. The code cannot access memory outside its allocation because the hardware MMU prevents it. The WASM module cannot write to the file system because the WASI interface does not expose that capability. The browser extension cannot read another tab's content because the process isolation model prevents it. These boundaries hold regardless of what the code tries to do, regardless of how it was written, regardless of social engineering. The constraint is structural.

A **Semantic security boundary** is enforced at the logical instruction layer. "Do not click external links." "Do not enable macros in documents from outside the organisation." "Your bank will never ask for your password." These are correct instructions. They require the user to maintain a rule in their head, apply it correctly under every circumstance, and resist social pressure, time pressure, and the ambiguity of "external" in a world where everything is connected.

The user is the sandbox. The user is not a reliable sandbox.

---

## WASM: The Sandbox That Holds

WebAssembly runs in a capability-constrained environment by design. The runtime defines a precise set of capabilities the module can use — nothing more, nothing less. Memory access is bounded to the module's linear memory. System calls are mediated through the WASI interface and only those explicitly granted are available. Network access requires explicit host permission. File system access is scoped to specific directories if granted at all.

The sandbox is not a policy. It is a structural property of the execution model. The malicious WASM module that tries to read your SSH keys cannot — not because it was told not to, but because the capability was never granted and the runtime enforces the absence of capability at the Gutenberg layer.

When something escapes a WASM sandbox it is a CVE — a genuine implementation flaw in the runtime itself. This is rare, expensive to find, and treated as critical precisely because the entire security model depends on the boundary holding. The community responds immediately because the escape is the exception that proves the rule. The rule is that the sandbox holds structurally.

The cognitive tax on the user: zero. The user does not think about the WASM sandbox. They run the application. The Gutenberg layer does its job quietly, invisibly, structurally. No warnings. No policies. No "please be careful."

---

## The Policy That Cries Wolf

The Office "this document is from the internet" warning and the IT policy "do not click external links" are Semantic instructions dressed as Gutenberg boundaries. They look like protection. They are reminders.

The yellow Protected View bar in Office looks like a system-level constraint. It is a policy reminder. The moment the user clicks "Enable Editing" or "Enable Content," the document can do everything a local document can do — execute macros, phone home, access the file system within the user's permissions. The Gutenberg layer is not constrained. The user was simply warned. The warning is the entire security model.

The IT policy "do not click external links" asks the user to:
- Identify every link before clicking
- Evaluate whether it is internal or external
- Evaluate whether the external destination is trustworthy
- Resist social engineering that creates urgency, authority, or familiarity
- Do this correctly, every time, under every circumstance

This is the maximum possible cognitive tax applied at the moment of maximum pressure — when someone is urgently asking you to verify an invoice, confirm a delivery, or reset a password. The policy was designed for the 1% dangerous case. The user has learned from the 99% benign case. The threat model calibrates toward "probably fine" because it almost always is. Until it is not.

**The warning that cried wolf** makes this worse. The Office warning appears for every document downloaded from the internet — including the completely legitimate documents that constitute the overwhelming majority of office work. The VSCode "do you trust the authors?" dialog appears for every repository cloned from GitHub — including the thousands of legitimate repositories cloned daily without incident. The signal-to-noise ratio is so low that users learn to dismiss without reading. The warning exists. The warning is invisible. The Gutenberg layer is still wide open.

---

## The Comparison

| | WASM sandbox | "Don't click links" policy |
|---|---|---|
| Where the boundary lives | Gutenberg layer (runtime) | Semantic layer (user judgment) |
| Enforcement | Structural — cannot be bypassed | Instructional — can be ignored or fooled |
| Failure mode | CVE — implementation flaw, rare | Phishing click — social engineering, constant |
| Cost of failure | Patch the runtime | Ransomware, credential theft, breach |
| User cognitive tax | Zero | Maximum — evaluate every link |
| Attacker's required effort | Find a runtime zero-day | Send a convincing email |

The WASM model puts the complexity where it belongs — in the runtime, paid once, maintained by experts, improved over time. The policy model puts the complexity where it does not belong — in the user, paid on every click, degrading under pressure, failing predictably at scale.

The attacker who needs to find a WASM runtime zero-day faces a high technical barrier and a narrow window — the patch arrives quickly and is deployed uniformly. The attacker who needs to send a convincing email faces a low barrier and a permanent window — the policy is not patchable, the user is not uniformly trained, and the social engineering techniques improve faster than the awareness training.

---

## The Correct Fix Is Always Gutenberg

Every security improvement that has actually worked at scale moved the boundary to the Gutenberg layer:

**Process isolation** — browser tabs cannot access each other's memory because the operating system's process boundary prevents it. Not a policy. A structural separation.

**Remote browser isolation (RBI)** — the page executes in a cloud container. The user sees a pixel stream. The malware cannot reach the endpoint because the endpoint is not in contact with the page. Gutenberg separation: the execution environment and the user's machine are physically different systems.

**Zero-trust network architecture** — removes the assumption that "internal network = trusted." Every connection requires authentication regardless of where it originates. The boundary is not the firewall perimeter (a Semantic boundary: "be careful outside") but cryptographic identity (a Gutenberg boundary: "prove who you are for every connection").

**Email sandboxing** — links in emails are first resolved in an isolated cloud environment. The user's browser only opens after the destination has been evaluated. The Gutenberg resolution happens before the Semantic commitment.

Each of these costs money and engineering. Each is more reliable than awareness training because each is structural rather than instructional. The user's judgment is removed from the critical path — not because users are incompetent, but because structural boundaries do not have bad days, do not click under pressure, and do not calibrate toward "probably fine."

---

## The Document Warning Done Right

The Office "this document is from the internet" warning is the worst of both worlds: a Semantic instruction that looks like a Gutenberg boundary, shown so frequently that users click through without reading, providing no actual structural constraint.

The honest version would be modelled on WASM capability grants:

*"This document requests the following capabilities that your policy restricts:*
- *Execute macros: BLOCKED*
- *Access external network resources: BLOCKED*
- *Read files outside the document: BLOCKED*

*The document has been opened with these capabilities disabled. To enable specific capabilities, contact IT with reference [ID]."*

Specific. Enumerable. Requiring explicit approval for each capability rather than a single "Enable Content" button that grants everything. The user sees exactly what is being prevented — the Gutenberg constraints made visible rather than hidden behind a yellow bar that says "be careful."

This is the [waterline made visible](https://rinie.github.io/2026/06/08/hiding-the-waterline/) applied to security. Not hiding the constraint. Not pretending the constraint is stronger than it is. Showing exactly where the Gutenberg boundary is and what it is preventing.

---

## Auto Mode and the Real Risk

The real risk in WASM is the sandbox escape — a zero-day that breaks the Gutenberg boundary. This is the scenario that the WASM security community works hardest to prevent and responds to most urgently when it occurs. The entire value of the model depends on the structural guarantee holding. When it fails, the failure is visible, attributable, and patchable.

The real risk in "do not click links" is different: the policy degrades continuously, invisibly, without any visible failure event. No CVE is filed when a user clicks a phishing link. The policy failure is distributed across thousands of individual decisions, each one invisible, each one accumulating into the breach that eventually becomes visible. By then it is too late and the breach cannot be attributed to any single policy failure.

The WASM escape is dramatic and rare. The policy failure is quiet and constant. The security model that produces dramatic rare failures is more manageable than the one that produces quiet constant ones — because dramatic rare failures get patched.

Quiet constant failures get awareness training.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [The Boundary Has a Lifecycle](https://rinie.github.io/2026/05/26/boundary-lifecycle/) on WASM as the endpoint of boundary evolution, [Ambiguity Is Not a Bug](https://rinie.github.io/2026/06/22/ambiguity-is-not-a-bug/) on the resolver that honestly surfaces uncertainty, and [Hiding the Waterline Makes You Drown Without Knowing Why](https://rinie.github.io/2026/06/08/hiding-the-waterline/) on the cost of invisible boundaries.*
