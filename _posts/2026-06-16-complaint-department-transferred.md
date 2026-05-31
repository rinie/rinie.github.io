---
layout: post
title: "The Complaint Department Has Been Transferred to Another Dimension"
date: 2026-06-16
tags: [enshittification, def-push, resolver, platform, use-pull]
level: general
description: "Enshittification is what happens when the platform that built a clean external resolver discovers the resolver is an extractable resource. The Sirius Cybernetics Corporation's complaint department was the first to be transferred to another dimension. Google, Amazon, Apple, and Meta are following the same arc. The structural fix is always the same: open resolvers, portability, and competition."
---

The Sirius Cybernetics Corporation, according to the Hitchhiker's Guide to the Galaxy, manufactured robots and mechanical creatures that were "Your Plastic Pal Who's Fun to Be With." Their marketing division also invented the phrase "Share and Enjoy," which they printed on all their products despite the products making life measurably worse for everyone who used them.

In the Guide, the Sirius Cybernetics Corporation is the first against the wall when the revolution comes — stated by Adams as a straightforward fact, in the same tone you might note that rain makes things wet. The revolution is not presented as desirable. It is presented as the inevitable outcome of eliminating the feedback loop entirely. When the complaint department is transferred to another dimension, when "Share and Enjoy" is the only response to genuine suffering, when the Def has no mechanism left for hearing the Use signal — eventually something forces the feedback loop open. In the Guide that something is a revolution. In technology it is usually a competitor, a regulator, or a critical mass of users quietly leaving.

The Sirius Cybernetics Corporation is not a metaphor. It is a roadmap.

---

## Enshittification: The Lifecycle

Cory Doctorow named the pattern in 2023 but the pattern predates the name by decades. Every platform business that builds a clean external resolver — a system that maps user intent to useful results — goes through the same arc:

1. **The resolver works.** The platform is genuinely useful. Users arrive because the product is good.
2. **The resolver is monetised.** Sponsored results appear above organic ones. The fee is small and the organic results are still good. Users accept the trade.
3. **The resolver is captured.** The sponsored results expand. The organic results are buried. The fee rises. The resolver now serves the platform's interests more than the user's.
4. **The resolver is extracted.** The platform extracts maximum value from both users and business customers simultaneously. Quality continues to decline. Users have no exit because the network effects that made the platform valuable also make leaving expensive.
5. **The complaint department is transferred to another dimension.** The feedback loop is closed. The Use signal has nowhere to go. The platform has become the thing it was built to replace.

---

## Four Resolvers and What Happened to Them

**Google** built the cleanest external resolver for the web. PageRank derived semantic relevance from Gutenberg link structure — what pages link to what, interpreted as a vote for quality. No ads above the fold in 1999. Results ranked by what users actually wanted to find. "Don't be Evil" was the policy statement for a company that had built something genuinely useful and wanted to stay that way.

By 2024 the first page of Google search results is predominantly ads, Google's own properties, and SEO-optimised content designed to rank rather than to inform. The resolver still works — well enough that alternatives have not displaced it. But the waterline between "what Google wants you to see" and "what is most relevant to your query" is muddier every year. "Don't be Evil" was quietly dropped from the corporate code of conduct in 2018.

**Amazon** built a resolver for retail: recommendations, reviews, search. In 1999 "Customer Obsession" meant the resolver served the shopper. Finding the right product at the right price was the mission. The reviews were real. The search results were ranked by relevance.

By 2024 sponsored products dominate search results. Fake reviews are endemic and Amazon's own detection systems are visibly losing the arms race. Amazon's own-brand products appear prominently in results for searches that should surface competitors. Third-party sellers pay fees that have increased steadily while the terms of service have tightened. Prime has been repriced upward while the delivery window has quietly stretched. "Customer Obsession" is still the first Leadership Principle. The practice has drifted toward "Platform Extraction Optimisation."

**Apple** built a resolver for software: the App Store. In 2008 it was a genuine improvement over the previous state of mobile software distribution — a curated, safe, searchable catalogue of applications for a new kind of device. The 30% fee was presented as the cost of the platform and the security review.

By 2024 the 30% fee is a toll booth on the entire iOS software economy. Sideloading — installing software from outside the App Store — is prohibited on the grounds of security, though the real effect is to maintain Apple's position as the sole resolver for iOS software. The EU Digital Markets Act is forcing partial compliance with interoperability requirements that Apple is implementing in the narrowest possible way while publicly arguing against them. The products remain excellent. The platform is a walled garden with rent extraction built in and the gate controlled by one company.

**Meta** ran the fastest enshittification arc. Facebook was genuinely useful for staying connected with friends and family from 2006 to roughly 2012. The feed showed you what your friends posted, in order. The resolver was simple and honest: you followed people, you saw what they shared.

The pivot to algorithmic feeds happened so gradually that users barely noticed each individual step. By 2024 the feed is primarily content from pages and accounts you did not choose to follow, interspersed with ads targeted by data you did not knowingly provide, optimised for engagement rather than connection. The resolver — what you see — no longer serves the Use signal of what you wanted to see. It serves the platform's need for time-on-site. The complaint department is in another dimension. The Genuine People Personality says "we're helping you stay connected."

---

## Andy Rubin and the Human Scale

Andy Rubin built Android as a genuine Use-Pull platform — open, licensable, the counter to Apple's closed garden. The openness was real. Any manufacturer could ship Android. Any developer could publish without a review gate. The platform fragmented in ways that served specific use cases precisely because no single entity controlled the resolver.

Rubin was nicknamed "Android" by colleagues because of his enthusiasm for robots. He built the most widely deployed operating system on Earth. A genuine achievement in the service of genuine openness.

Then the power corrupted — not at the platform level but at the human level. Rubin left Google in 2014 following a sexual misconduct investigation. Google paid him a $90 million exit package and said nothing publicly for four years. When the story emerged in 2018 it became a case study in institutional Def-Push: the Use signal (the people harmed) was suppressed, the architect's departure was managed to protect his reputation, and the feedback loop was deliberately closed. The payment was the price of silence.

"Don't be Evil" was a promise about the resolver. It was also a promise about the feedback loop — that the institution would hear the Use signal even when the Use signal was inconvenient. The $90 million was the price of discovering that the promise applied only when the Use signal was commercially safe to hear.

Android survived because the Gutenberg layer — the open OS, the published API, the licensable platform — was never owned by one person. The semantic layer, the culture, the accountability, was more fragile.

---

## The Structural Fix

The enshittification arc is not inevitable. It is the predictable outcome of a closed resolver with no competitive pressure and no portability mandate. The structural fixes are the same ones that worked for phone numbers, for email hosting, for music formats:

**Open protocols.** DNS works because no single entity owns it. ActivityPub, email federation, RSS — open resolvers that no single company can enshittify because no single company controls them. The web works at scale because HTTP and HTML are open. The moment a platform captures the protocol, the enshittification clock starts.

**Portability mandates.** Number portability was not delivered by the market — carriers had every incentive to maintain the coupling. It was mandated by regulators because the Use signal (people locked into bad carriers by the cost of losing their number) could not reach the Def through normal market mechanisms. The same mandate applied to bank accounts, social graphs, and music libraries would have the same effect.

**Competition.** Lidl kept Albert Heijn honest. The market fix when it works — when switching costs are low enough that the Use signal can travel through exit.

**Own the resolver.** Your own domain for email. Your own server for your data. Your git remote on infrastructure you control. The individual fix when market and regulation both fail.

The Sirius Cybernetics Corporation transferred the complaint department to another dimension because they could — because there was no mechanism to stop them, no competitor to defect to, no regulator to mandate a feedback loop. The revolution in the Guide is absurdist comedy. In technology the equivalent is quieter: users drift away, alternatives emerge, the platform that eliminated the feedback loop discovers that the feedback loop was load-bearing after all.

The complaint department should not be in another dimension. It should be at the waterline — visible, accessible, and honest about what it hears.

Share and Enjoy.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Competition Is Use-Pull. Monopoly Is Def-Push. Government Is Both.](https://rinie.github.io/2026/05/29/competition-use-pull-monopoly-def-push/) on exit as the Use signal transmission mechanism, [Your Email Address Is Hostage](https://rinie.github.io/2026/05/25/email-address-hostage/) on owning your own resolver, and [Gutenberg: Your Next Phone Will Be a Different Make on a Different Carrier. Your Number Is Still Yours.](https://rinie.github.io/2026/06/15/gutenberg-your-next-phone/) on what a working resolver looks like.*
