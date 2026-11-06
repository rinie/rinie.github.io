---
layout: post
title: "The 72-Hour Window, Measured: Nothing Installs Until It's a Week Old"
date: 2026-11-06
tags: [gutenberg-semantic, supply-chain, npm, cooldown, last-known-good, dumb-wires, meterkast]
level: technical
description: "Two earlier posts here proposed holding a new package version back for 72 hours and gave no source for the number. A company now runs a seven-day version through a proxy and published the figures: five 2026 hijacks, all pulled within five hours, and 71 refused downloads in twelve days as the price. The clock starts at publish, looks at nothing inside the package, and holds against builds that carry valid provenance."
---

Two earlier posts here, [Ambiguity Is Not a Bug](https://rinie.github.io/2026/06/22/ambiguity-is-not-a-bug/) and [It Is Always DNS](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/), proposed holding a new version back for 72 hours and said most hijacks surface inside that window. Neither had a source for the number. Bruno Jurković has now published the production version of the idea, [a seven-day gate in front of npm and PyPI](https://brunoj.com/nothing-installs-at-our-company-until-its-a-week-old/), with twelve days of figures behind it.

---

## What the Window Looks Like When Someone Measures It

He lists five 2026 hijacks that came with proper timelines and records how long each poisoned version stayed up: axios 1.14.1 about three hours, lightning on PyPI 42 minutes, TanStack 4 hours 35 minutes, keyv about 1 hour 40 minutes, litellm 1 hour 43 minutes. All five were pulled within five hours. Detection came from outside the installers: a security vendor opened an issue on TanStack's repo 26 minutes after the first publish, and Lightning's own community raised the alarm within 42 minutes. The damage lands before that, since a fresh install can pick up a new version within seconds. Huntress logged its first axios infection 89 seconds after publish.

So 72 hours covers every incident in the sample with a wide margin, and the gate sets seven days anyway. The incident windows are measured. The seven days is a margin chosen against the tail, and the article lists what stays outside it: the xz backdoor took 34 days to find, and a hijacked npm package was still tagged `latest` 228 days after it went up. A version nobody notices for a week gets in on day eight.

---

## The Clock Starts at Publish

The gate hides any version younger than seven days from the version list. The package manager does what it always did and picks the newest version it is offered, which is now the newest one that has survived the wait. Nothing on the client changes except the registry URL. A lockfile that pins a version the gate won't serve yet gets a 404, which is how 71 of 81,710 downloads were refused in twelve days.

That is last known good, derived instead of stored. The gate keeps no record of what it has seen. The age comes from the publish date the registry already holds, so the clock starts when the package was published, which is where the attacker's exposure starts, and it works on a machine that has never seen the package before. A client that had to remember first sight would start the clock late and lose it on a fresh checkout.

---

## A Dumb Check Beats a Smart One Here

The sharpest line in the article is StepSecurity's, on why signed builds don't help. The malicious TanStack and keyv versions came out of the projects' own release workflows, with valid provenance attached: "Provenance proves which commit was built. It cannot prove the commit was authorized."

Attestation answers a smart question, was this built the way it claims, and the attacker was standing inside the build. A clock answers a dumber one, and nothing satisfies it except waiting. The gate never looks inside a package. It reads one field. That makes it [a dumb wire](https://rinie.github.io/2026/09/05/in-praise-of-dumb-wires/) in the series' sense: one input to get wrong, no opinion about content, and a cost disclosed in advance.

The confirmation signal had to move as well. The staged rollout in the DNS post watched whether the new value worked. A poisoned package is built to install cleanly, so success on the installing machine confirms nothing. The only signal that doesn't need the attacker's cooperation is somebody else noticing, and the cheapest way to buy it is to wait. The article says the same about the wait itself: it buys time for someone else.

---

## One Door, One Meter

The author chose a proxy over the per-tool setting because a setting protects only the repo it lives in, and a forgotten Dockerfile or a colleague's laptop never reads it. The settings also come in eight spellings of one idea: days in one tool, minutes in another, seconds, `1w`, an ISO duration. The proxy collapses that to one config line at one door, and the self-hosted CI runners aren't allowed around it.

That door is [a meterkast](https://rinie.github.io/2026/08/03/the-meterkast-pattern/): the one place where public meets private, with a meter on it. The download log ties every pull to an account, so "who already installed it?" is one Grafana query instead of a chat thread. A blocklist entry turns a version into a 403 everywhere, cached copies included. No setting in any client can say who already pulled the thing.

It also pays the cost [the four-shapes post](https://rinie.github.io/2026/09/10/four-shapes-a-boundary-can-take/) named for any hourglass: one waist everything passes through is one point that can't fail. If the gate is down, CI can't install anything, because the runners can't go around it. The author did that on purpose and says so, which is what separates a disclosed cost from a hidden one.

---

Twelve days: 81,710 downloads, 6,116 versions hidden from version lists, 71 refused, nothing on the blocklist. The price was a few 404s. The coverage was every incident in the sample, with days to spare. The edge is the xz kind of case, and the article marks it.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [It Is Always DNS](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/) on last known good and the original 72-hour window, [In Praise of Dumb Wires](https://rinie.github.io/2026/09/05/in-praise-of-dumb-wires/) on checks that don't look inside, and [The Meterkast Pattern](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) on the one door with a meter on it.*

Source: ["Nothing Installs at Our Company Until It's a Week Old"](https://brunoj.com/nothing-installs-at-our-company-until-its-a-week-old/), Bruno Jurković, brunoj.com, November 1, 2026. Incident timelines and the twelve-day figures are the author's; the StepSecurity line is quoted as it appears there.
