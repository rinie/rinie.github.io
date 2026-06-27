---
layout: post
title: "It Is Always DNS: The Version Chain Nobody Built"
date: 2026-07-26
tags: [gutenberg-semantic, dns, trust, resolver, security, versioning]
level: general
description: "DNS changes are all-or-nothing. The old value is gone the moment the new one propagates. There is no staged rollout, no last known good, no revert. A hijacked record looks identical to a legitimate update. None of this is inevitable — it is a design choice that was never revisited."
---

There is a running joke in infrastructure work: the answer to every mysterious outage is DNS. Certificate expired — DNS. Service unreachable — DNS. Deploy went sideways — DNS. The joke persists because DNS is the resolver that everything else depends on, and DNS failures are invisible until they are catastrophic. The Gutenberg layer (the IP address) and the Semantic layer (the hostname) diverge silently. The resolver shows no ambiguity. The user gets a white screen.

The deeper problem is not that DNS breaks. It is that when DNS changes — legitimately or not — there is no staged rollout, no last known good to fall back on, and no feedback loop to tell the resolver whether the new value is actually working. A hijacked record looks identical to a legitimate update. The resolver switches silently and completely.

None of this is inevitable.

---

## The All-or-Nothing Switch

When a DNS record changes, every resolver that asks gets the new value. There is no canary. There is no "send 10% of traffic to the new address and watch for errors." There is no revert button. The old value is gone the moment the TTL expires and the resolver fetches fresh data.

Software deployments learned this lesson the hard way and built staged rollout as standard practice. Deploy to one server first. Watch for errors. Expand if it looks good. Roll back if it does not. The old version stays available until the new one is confirmed working. The window between "deployed" and "confirmed working" is where the safety net lives.

DNS has no equivalent. The change goes out. The change takes effect. If something went wrong — misconfiguration, expired certificate on the new server, or an outright hijack — the resolver does not know. It switched. It is serving the new address. The user is getting a white screen and the resolver has already forgotten what the old address was.

---

## Last Known Good as the Safety Net

The fix does not require changes to the DNS protocol. It requires resolvers to remember more than they currently do.

A resolver that stores last known good — the value that was confirmed working last time — can stage the transition rather than switching blindly. When a record changes:

1. Serve the new value
2. Watch whether the connection actually succeeds — TLS completes, the service responds correctly
3. If yes: new value confirmed, promote it to last known good, close the ambiguity window
4. If no: revert to last known good, flag the new value as suspect

No blackout. The user does not see a white screen while the resolver figures out whether the change was legitimate. The old value covers traffic until the new one earns its place.

This is exactly how a good deployment pipeline works. The difference is that deployment pipelines were built by people who had watched production outages and added the safety net. DNS resolvers were built for a cooperative network where the question "what if this change is wrong?" was not in scope.

---

## Feedback Without Asking Anyone to Fill In a Form

The confirmation signal does not need to come from the user. It comes from the connection itself.

Outage-reporting sites like allestoringen.nl work because many users independently reporting "it's not working for me" aggregate into a signal faster than any official status page. The same logic applied one layer down, at the resolver, removes the cognitive tax from the user entirely. The resolver watches whether connections to the new address succeed or fail. It does not ask. It observes.

A new value that accumulates confirmed working connections within 72 hours is almost certainly a legitimate update — real infrastructure migrations get confirmed quickly across many resolvers. A new value that produces connection failures, certificate mismatches, or sits unconfirmed within 72 hours is almost certainly not. Most DNS hijacks become detectable within that window: the wrong server, the certificate that does not match, the service that does not respond as expected.

The 72-hour window is the key parameter. Last known good covers traffic during that window. Confirmed working closes it. Unconfirmed or failing reverts it. The pattern self-distinguishes without requiring anyone to intervene.

---

## Why Nobody Built It

DNS was designed in 1983 for fast lookup in a cooperative network. The resolver's job was defined precisely: return the current answer, quickly. Storing last known good, watching connection outcomes, staging transitions — all of this is outside that definition. Not because it is hard. Because the threat model was misconfiguration, not hijack, and the use case was a network where participants were assumed to be cooperative.

The assumption has not been true for decades. The definition of the resolver's job has not been updated to match.

The cost of adding last known good is small: a little local storage per name, a confirmation check on each successful connection, a slightly slower silent switch when records change legitimately. The cost of not having it falls on the users who get a white screen when a record changes and the new destination turns out to be wrong — whether by accident or by design.

It is always DNS. The resolver that remembers where it came from would make that a shorter conversation.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Ambiguity Is Not a Bug](https://rinie.github.io/2026/06/22/ambiguity-is-not-a-bug/) on resolvers that hide uncertainty instead of surfacing it, [Why IPv6's Def-Push Failed](https://rinie.github.io/2026/05/28/why-ipv6-def-push-failed/) on load-bearing boundaries that look like hacks, and [UUIDs Are Not Names](https://rinie.github.io/2026/05/27/uuids-are-not-names/) on the gap between Gutenberg identifiers and Semantic identity.*
