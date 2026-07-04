---
layout: post
title: "Ambiguity Is Not a Bug: Trust, Provenance, and the Resolver That Cried Wolf"
date: 2026-06-22
tags: [gutenberg-semantic, resolver, trust, security, ambiguity, npm, dns]
level: technical
description: "The resolver that presents false certainty is more dangerous than the resolver that honestly says 'I'm not sure.' WhatsApp fraud, npm hijacking, DNS propagation gaps, QR codes, SMS links — all the same problem: the Gutenberg state changed, the Semantic layer did not update, and the resolver showed no ambiguity. The user was taken for a ride."
---

Your contact list shows "Mrs Kervel" calling. You answer. It is a stranger — Mrs Kervel sold her old SIM six months ago. The resolver (your contacts app) presented confident Semantic identity over a Gutenberg identifier that no longer mapped to the person you knew. The confidence was false. The ambiguity was real. Nobody told you.

This is not a security failure. It is a resolver design failure. The system that should have shown "Mrs Kervel (number may have changed)" showed "Mrs Kervel" and let you be taken for a ride.

Ambiguity is a fact of life in any distributed system where Gutenberg identifiers (numbers, addresses, package names, domain names) and Semantic identities (people, organisations, software) evolve independently. The resolver that hides that ambiguity behind confident presentation does not make the system safer. It moves the risk from the resolver to the user — without telling the user the risk was moved.

---

## The WhatsApp New Number Fraud

"Hey it's me, I got a new number, save it."

The message arrives from an unknown number. The claim is Semantic — it's me, a person you know. The Gutenberg identifier is new, unlinked to any prior trust relationship, with no verified connection to the person claimed.

The fraud works because messaging apps present Semantic identity over unverified Gutenberg identifiers. The new number has no history in your contacts. The contact app shows no connection. The messaging app shows no warning. The user is left to evaluate a bare Semantic claim with no Gutenberg signal to check it against.

**The Use-Pull fix that would actually work:**

A new number claiming a known identity could trigger a confidence signal rather than a forced choice. Not "this is suspicious, block it" — that would generate false positives for everyone who genuinely changed phones. Not "trust it" — that is the current broken default. But: "new number, no prior contact history — the claimed identity is unverified."

Better still: **location context as a soft signal.** A new number from the same country, same carrier prefix, same rough geography as the existing contact record is more likely to be a genuine change. A new number from a different country, a VOIP prefix, or a geography inconsistent with the contact's history is worth flagging — not blocking, not rejecting, but flagging with visible ambiguity. "Hey it's me — but this number is registered in a different country than your contact. Verify before trusting."

This is more useful than "your bank never calls you." That instruction asks the user to maintain a rule in their head at the moment of highest social pressure — when someone is on the line claiming urgency, claiming authority, claiming to be someone trusted. The cognitive tax is maximum precisely when the fraud is most likely. The resolver that surfaces the Gutenberg anomaly (new number, foreign prefix, geography mismatch) removes the cognitive tax from the moment of pressure and puts it where it belongs — in the system that has the data.

---

## SMS Links and QR Codes: The Unvalidatable URL

The browser does something important with URLs that SMS and QR codes do not: it shows you where you are going before you commit.

A link in a browser is inspectable. Hover over it and the status bar shows the destination. The URL is visible. The domain can be checked. The HTTPS padlock can be verified. The browser's Gutenberg layer (the URL) is exposed to the user's Semantic layer (their judgment about whether to trust it) before the navigation happens.

An SMS link is not inspectable in the same way. The display text can say "click here to confirm your delivery" while the URL resolves to `parcel-track-nl-confirm.suspicious.ru`. The Semantic layer (delivery confirmation) and the Gutenberg layer (the actual destination) are deliberately decoupled. The user sees the Semantic claim without the Gutenberg evidence.

A QR code is worse — the Gutenberg layer (the URL encoded in the squares) is completely invisible until scanned. The user commits to the resolution before they can evaluate it. The QR code is a deliberate opacity layer between the Semantic intent ("scan to pay") and the Gutenberg destination ("your payment goes here").

**The stable destination problem:**

Even when the URL is visible, SMS links and QR codes often use URL shorteners — `bit.ly/xyz123`, `t.co/abcdef` — which hide the final destination behind a redirect. The short URL is stable. The destination is not. A QR code printed on a restaurant menu in 2023 that pointed to the menu PDF still points to the same short URL in 2026 — but the short URL may have been reconfigured to point somewhere else. The Gutenberg identifier (the QR code, the short URL) appears stable. The Semantic destination has moved. The resolver presents no ambiguity.

The browser's URL bar is the waterline made visible. It shows you the Gutenberg address you are about to cross to. SMS links and QR codes hide the waterline. The user is taken to a destination they could not evaluate before arriving.

**The Use-Pull fix:** show the resolved URL before navigation. QR code scanners that preview the destination URL before opening it — several exist, none are default. SMS apps that expand short URLs and show the final destination on hover. The Gutenberg layer made visible before commitment, not after. The waterline shown, not hidden.

---

## The npm Hijack: When the Package Name Stays Stable

The npm supply chain attack exploits the same resolver gap at the software layer.

A package name (`left-pad`, `event-stream`, `colors`) is a Semantic identifier. The specific version of code in the registry is the Gutenberg artifact. The resolver (npm) maps the name to the current version and presents the result confidently. `npm install colors` — here is colors.

The attack: the maintainer transfers the package to a new owner, or publishes a new version, or the account is compromised. The Semantic name is stable. The Gutenberg artifact it resolves to has changed. The resolver shows no ambiguity. The developer's `package.json` says `colors` — they get whatever `colors` currently is, not what `colors` was when they last verified it.

**The 72-hour window:**

Most supply chain attacks become detectable within 72 hours — someone notices the behaviour change, the registry flags the version, the security community alerts. But in those 72 hours every `npm install` resolves the compromised package with full confidence. The resolver's job — map name to artifact — is technically correct. The artifact changed. The confidence is false.

**Last known good, not a cryptographic chain:**

The correct fix is the same pattern described [later in the series](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/): a resolver that remembers what was working last time and stages the transition rather than switching blindly. Not a hash chain back to genesis — just last known good, the current value, and a confirmation loop that watches whether the new value actually works.

`npm install colors` with a 3-day-old maintainer transfer could show: "⚠ This package changed maintainers 3 days ago. Last verified version: 1.4.0. Current version: 1.4.1. Do you want to review the diff before installing?"

Not blocking. Not forcing. Surfacing. The user drives. The resolver shows the map.

---

## The Warning That Cried Wolf

The Office "this document is from the internet" warning and VSCode's "do you trust the authors of this folder?" are attempts to surface provenance ambiguity. Technically correct. Practically invisible.

They are shown so frequently for benign cases — every document downloaded from a legitimate source, every repository cloned from GitHub — that users click through them reflexively. The Gutenberg signal (provenance uncertain) is present. The Semantic signal (this specific thing may be dangerous) is absent. The warning cannot distinguish between "benign download from trusted source" and "compromised package from hijacked account." Both get the same dialog. Users learn to dismiss both.

This is the resolver crying wolf. The warning system that treats all ambiguity as equally concerning produces users who treat all warnings as equally ignorable. The cognitive tax of reading and evaluating every warning exceeds the cognitive budget of most users doing routine work. The warning becomes noise. The ambiguity it was meant to surface is hidden again — not by false confidence but by warning fatigue.

**The Use-Pull fix:** calibrated signals, not binary alerts.

- ✓ **Known good** — previously verified, no changes since, consistent provenance
- ℹ **Routine** — from internet but consistent with expected source, no anomalies
- ⚠ **Changed recently** — maintainer transfer, new certificate, geography mismatch
- 🚨 **Anomaly** — significant departure from established pattern, verify before proceeding

The `⚠` and `🚨` states earn attention because the `✓` and `ℹ` states are quiet. The resolver that shows confidence levels rather than binary safe/unsafe gives the user a calibrated map rather than a constant alarm.

---

## The AI Training Cutoff: Stale Certainty

The same pattern appears in AI assistants. The training cutoff is a Gutenberg fact — the model's knowledge ends at a specific date. The answers are presented at a Semantic confidence level that does not reflect the Gutenberg staleness.

Ask about a DNS configuration that changed last week. Ask about a package that was compromised three days ago. Ask about a number portability rule that changed last month. The model answers from its last known state — confidently, helpfully, incorrectly. The Gutenberg state (what is true now) diverged from the Semantic confidence (what the model believes). The resolver showed no ambiguity.

The Use-Pull response: surface the uncertainty. "As of my training data, X — but this type of information changes frequently and may have been updated since." Not refusing to answer. Not pretending certainty. Showing the confidence level alongside the answer so the user can calibrate.

The 72-hour supply chain attack window, the DNS propagation delay, the AI training cutoff, the QR code pointing to a stale destination — all the same structural problem. Gutenberg state is ephemeral. Semantic identity claims to be stable. The gap between them is where the fraud lives, where the hijack happens, where the stale answer misleads.

---

## The Resolver That Serves the User

The common thread in every case is the same: the resolver that presents confident results when the underlying data is ambiguous, stale, or unverified is not serving the user. It is serving its own appearance of competence.

The honest resolver:
- Shows confidence levels, not just results
- Flags recent changes, maintainer transfers, geography mismatches, staleness
- Does not force merges or choices — shows the ambiguous state and lets the user decide
- Treats "I'm not sure" as a valid answer, not a failure state
- Reserves strong warnings for genuine anomalies so they are actually read

The WhatsApp fraud, the npm hijack, the DNS propagation gap, the QR code pointing somewhere unexpected, the AI training cutoff, the Office warning nobody reads — all resolvable not by eliminating ambiguity (you cannot) but by representing it honestly.

The user should drive. The resolver shows the map. When the map has a question mark on it, the map should show the question mark — not fill it in with the most confident-looking answer available and hope nobody notices.

Ambiguity is not a bug. Hiding it is.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [UUIDs Are Not Names](https://rinie.github.io/2026/05/27/uuids-are-not-names/) on the Mum/Mrs Kervel many-to-one problem, [It Is Always DNS: The Version Chain Nobody Built](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/) on last known good as the right default, and [Hiding the Waterline Makes You Drown Without Knowing Why](https://rinie.github.io/2026/06/08/hiding-the-waterline/) on the cost of invisible boundaries.*
