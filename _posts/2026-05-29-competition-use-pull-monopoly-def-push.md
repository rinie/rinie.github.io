---
layout: post
title: "Competition Is Use-Pull. Monopoly Is Def-Push. Government Is Both."
date: 2026-05-29
---

This is part of a series. The [Def-Use split](https://rinie.github.io/2026/05/16/def-use-lean-pull/) established that the gap between what authors define and what users actually need is where most waste lives. The [boundary lifecycle post](https://rinie.github.io/2026/05/26/boundary-lifecycle/) showed how platforms manage — or fail to manage — the interface between their infrastructure and their users. This post asks: what happens when the Use signal has nowhere to go?

---

## 1. The Use Signal Needs an Exit

The Use-pull feedback loop only works if users can leave. When a product fails to serve its users, they switch to a competitor. The competitor gains market share. The original product either adapts or loses. The Use signal travels through exit: not through surveys, not through complaints, not through user research — through the actual behaviour of users choosing something else.

This is not a moral argument. It is a mechanical one. The Use signal requires a transmission mechanism, and exit is the most reliable one available. Without exit, the signal still exists — users still have unmet needs, frustrations, workarounds — but it cannot reach the Def side in a form that forces change. The Def optimises for its own convenience, not for Use.

Competition is the structural guarantee of exit. When Lidl entered the Dutch supermarket market, Albert Heijn could no longer assume its customers had nowhere else to go. The Use signal — price sensitivity, preference for variety, willingness to switch for quality — became audible because it was backed by actual switching behaviour. The supermarket had to listen because the alternative was losing customers to a competitor that was already listening.

---

## 2. Lidl Kept the Supermarket Honest

Before discount supermarkets arrived in force, Dutch supermarkets operated in a comfortable oligopoly. The Def (product range, pricing, store layout, opening hours) was set by the supermarket. The Use signal existed — customers wanted lower prices, more variety, less waste — but it was weak because switching was inconvenient and the alternatives were similar.

Lidl and Aldi changed the transmission mechanism. Their entry made exit genuinely easy. Customers switched, visibly and measurably. Albert Heijn and Jumbo had to respond: lower prices on staples, better private label ranges, more competitive fresh food sections. The Def moved toward Use not because the supermarkets became more virtuous but because competition enforced the feedback loop.

The supermarket "sux less" not because it cares more. It sux less because it cannot afford not to.

This is Use-pull enforced by market structure rather than by design. The same dynamic explains why VS Code improved faster than Eclipse, why Chrome improved faster than IE, why Android improved faster than Symbian. In each case a new entrant made exit easy, the Use signal became audible through switching behaviour, and the incumbent had to adapt or lose.

---

## 3. Platform Risk: When the Exit Gets Expensive

Platforms occupy an uncomfortable middle position. They start with competition — GitHub versus Bitbucket versus SourceForge, Gmail versus Hotmail versus Yahoo — and use that competition to attract users with good defaults, sensible pricing, and genuine Use-pull design. Then network effects, switching costs, and ecosystem lock-in progressively raise the cost of exit.

GitHub is the live example. The Gutenberg layer — git, the repo format, the protocol — is open. `git push` works against any remote. GitLab, Gitea, Codeberg, self-hosted: all accept the same bytes. The Use signal can still travel through exit at the Gutenberg layer: fork your repo, point your remote elsewhere, done in an afternoon.

But the semantic layer — Actions workflows, Pages configuration, Copilot integration, issue history, PR comments, team permissions — accumulates over time and raises the switching cost. Microsoft acquired GitHub in 2018. Copilot arrived as a paid feature in 2021, deeply integrated into the editing workflow. Each integration is genuinely useful and simultaneously a coupling between your semantic layer (your development process) and their Gutenberg carrier (GitHub's infrastructure).

The enshittification ceiling exists because git is the load-bearing fence. Microsoft can degrade the UI, raise prices, and bundle Copilot into workflows, but they cannot enshittify the bytes. The git protocol is open and the alternatives are real. This keeps the Use signal alive — not as strongly as a fully competitive market, but enough to set a floor on how bad things can get before mass exit begins.

The mitigation is the same as for email: own the semantic layer, make the carrier replaceable. Custom domain for Pages. Portable CI/CD scripts that run on any Actions-compatible platform. Treating GitHub as infrastructure, not as identity.

---

## 4. Government: No Exit by Definition

Government is the limiting case. There is no exit. You cannot take your tax obligations to a competing revenue service. You cannot choose a different passport issuer because the queue is shorter. You cannot switch to a rival municipality because the planning permission process is faster.

The Use signal exists — citizens have unmet needs, frustrations, workarounds in abundance. But it cannot travel through exit. It travels instead through elections (too slow and too aggregated to provide useful feedback on specific services), through complaints processes (captured by the institution being complained about), and through media coverage (intermittent and dependent on the complaint being dramatic enough to be newsworthy).

None of these transmission mechanisms are as efficient as exit. The Def side — the institution, the regulation, the legacy system — can ignore the Use signal for years, sometimes decades, without facing the mechanical consequence that a competitive market would impose.

This is not a criticism of the people who work in government. It is a structural observation about the feedback loop. A civil servant who wants to improve a service faces the same problem as a product manager at a monopoly: the Use signal is present but muffled, and the institutional incentives to respond to it are weaker than in a competitive market.

---

## 5. Sunday Shopping: YAGNI as Policy

For decades, Dutch shops were closed on Sundays by law. The justification combined religious observance, labour protection, and an implicit assumption about what people needed: they did not need to shop on Sunday. The Def (the regulation) had decided what Use required.

This is YAGNI applied to public policy. In software, YAGNI means: do not build features users have not asked for. Government YAGNI is the inversion: do not provide services users are asking for, because we have decided they do not need them. The arrogance is identical — a Def predicting what Use needs without a feedback mechanism to check whether the prediction is correct.

When Sunday trading was liberalised, demand was immediate and sustained. The Use signal had been there all along. It was suppressed by law, not by absence of need. The prediction was wrong. Without competition and without exit, the wrong prediction stood for decades.

The same pattern appears in digital government services. Online tax filing, digital passport renewal, evening appointments, single sign-on across government services: each was resisted, each was predicted to have low demand, each was immediately and overwhelmingly used when made available. The Def's model of what Use needed was consistently wrong. The Use signal was consistently ignored until the service launched and made the gap undeniable.

---

## 6. De-Mail and DigiD: The Government Digital Service Pattern

The German De-Mail system is the cleanest example of government Def-push in digital services. Designed to provide legally binding secure email, it was built around the Behörden's needs: Zustellfiktion (legal delivery at time of sending, not receipt) reduced administrative burden for the sending institution. For the user, adopting De-Mail degraded their legal position compared to paper Einschreiben — the physical registered letter with a signed receipt that proved delivery. The system punished adoption. Users who switched to De-Mail were worse off than users who stayed with paper.

The system was not used. Not because users did not want secure digital communication with government — they did — but because the Def imposed costs on the Use side to benefit the Def side. The absence of competition meant there was no correction mechanism. De-Mail was eventually discontinued.

The Dutch DigiD/iDIN/eHerkenning landscape is a less dramatic but equally instructive example. Multiple incompatible digital identity systems exist for different contexts: DigiD for citizens, eHerkenning for businesses, iDIN for banking identity. Each was designed around the issuing institution's Def. The user must maintain the mapping — which system for which service — because no single institution was responsible for making the user's experience coherent. This is the UUID leak problem from [post 13](https://rinie.github.io/2026/05/27/uuids-are-not-names/) applied to identity: the system hands Gutenberg artifacts (separate identity credentials) to users and expects them to manage the mapping themselves.

No competition means no pressure to unify or simplify. The Use signal — users wanting a single coherent identity system — is audible in every usability study and user complaint. It cannot travel through exit, so it travels slowly through inter-institutional negotiation instead.

---

## 7. The Regulator as Use-Proxy

When exit is impossible, regulation is the only mechanism for injecting the Use signal into the Def. The regulator functions as a Use-proxy: it represents the interests of users who cannot otherwise make their preferences heard through market behaviour.

Regulation works when it genuinely acts as a Use-proxy:

- **EU roaming regulation** forced carriers to treat connectivity as portable across national networks. The Use signal (people wanting their phones to work abroad without bill shock) was represented by the regulation when market competition could not deliver it.
- **Number portability** forced carriers to accept that your phone number belongs to you, not to them. The Use signal was represented by law when carriers had every incentive to suppress it.
- **Sunday trading liberalisation** removed a Def-push that had suppressed the Use signal for decades. The regulation stopped predicting what Use needed and let Use decide.

Regulation fails when it adds its own Def-push on top of the platform's:

- **GDPR cookie consent banners** are the clearest example. The regulation was designed to give users control over their data — a genuine Use-pull objective. The implementation produced a Def-push: every website now presents a consent dialogue designed by the website's legal team to maximise consent, not to genuinely inform users. The Use signal (people wanting control over their data) was captured by the Def (the consent banner industry) and turned into noise. The regulation added friction without delivering the benefit.

The difference between regulation that works and regulation that doesn't is whether it genuinely represents the Use signal or whether it is captured by the Def side and turned into compliance theatre.

---

## 8. Competition Is the Mechanism, Not the Goal

The argument here is not that markets are always right or that competition solves everything. It is narrower: **competition is the most reliable transmission mechanism for the Use signal.** When competition is absent — in government services, in natural monopolies, in platform ecosystems with high switching costs — the Use signal must be transmitted by other means, and those other means are consistently less efficient.

The structural fix is the same in every context: wherever exit is impossible or expensive, a Use-proxy is required. In markets, competition is the Use-proxy. In regulated industries, the regulator is the Use-proxy. In government, democratic accountability is theoretically the Use-proxy — but elections are too slow, too aggregated, and too disconnected from individual service quality to function as an efficient feedback mechanism for the specific Defs that affect daily life.

The practical implication for anyone building products or services: competition is not just a business threat. It is a gift. It keeps the Use signal alive, forces the feedback loop open, and prevents the Def from drifting into YAGNI arrogance. The product that faces no competition faces no mechanism for correction. It becomes, eventually, a government service — not through malice but through the structural absence of the exit that makes Use-pull possible.

Lidl did not make supermarkets virtuous. It made their vices expensive. That is enough.
