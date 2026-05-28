---
layout: post
title: "Your Email Address Is Hostage. It Does Not Have to Be."
date: 2026-05-25
category: "Data & Identity"
tags: [identity, email, data-portability, vendor-lock-in]
depth: conceptual
---

In the [previous post](https://rinie.github.io/2026/05/24/music-survived-six-formats/) we asked why your music survived six formats but your bank account number stayed behind. The answer was deliberate coupling — companies and institutions keeping your semantic identity locked to their Gutenberg carrier because the migration tax keeps you from leaving.

Email is the oldest and most personal example of the same pattern. And unlike music, where the market eventually forced the separation, and unlike phone numbers, where regulation forced it, email has never been fixed. The trap was built into the address itself on day one — and most people walked straight in.

---

## 1. The Address Is the Trap

Your email address has two parts: the part before the `@` and the part after it. The part before is yours. The part after belongs to someone else.

`rinie@xs4all.nl` — the `xs4all.nl` part is XS4All's domain. The day that domain stops working, or the day XS4All stops hosting email, your address stops working. Every contact who has ever emailed you, every service you have ever signed up for, every newsletter subscription, every password reset flow — all of it is tied to an address that someone else controls.

This is not a technical accident. It is how email was designed — federated, distributed, each domain owned by someone. The design is actually correct and elegant. The mistake was letting someone else own the domain in your address.

---

## 2. Three Generations of the Same Trap

**Generation 1: ISP email — `rinie@xs4all.nl`**

The original trap. Your email address was literally your internet provider's domain. Switch provider, lose address. This was so effective at preventing churn that many people paid above-market prices for decades rather than face the migration tax.

XS4All is a particularly poignant Dutch example. A beloved ISP with a fiercely loyal community, founded by hackers and activists, known for fighting for user privacy and digital rights. Acquired by KPN in 1998. Kept alive as a brand for twenty years by customer loyalty — loyalty that was partly genuine affection and partly the migration tax of losing `@xs4all.nl` addresses. KPN announced the end of XS4All as a brand in 2019. The coupling that kept users loyal protected neither the user nor the brand in the end.

The ISP email trap has no regulation equivalent to number portability. Nobody legislated that you could take `rinie@xs4all.nl` to a new provider when XS4All closed. The address simply stopped working and users had to rebuild their identity from scratch.

**Generation 2: Platform email — `rinie@hotmail.com`**

Hotmail launched in 1996 and offered something genuinely new: an email address that was not tied to your internet provider. You could switch ISP and keep your Hotmail address. The coupling was broken one level up — now your address was tied to a platform rather than a provider.

This was better. Hotmail, Yahoo Mail, and Gmail were more stable than ISPs and not tied to your physical connectivity. But the fundamental structure was the same: someone else owned the domain in your address.

Microsoft acquired Hotmail in 1997. In 2013 they migrated it to Outlook.com. The `@hotmail.com` addresses continued to work — Microsoft handled the transition gracefully — but that was Microsoft's choice, not yours. They could have discontinued the addresses. For millions of users who had built their identity around `@hotmail.com` there was no alternative but to hope Microsoft kept the lights on.

Yahoo Mail users discovered this the hard way through years of security breaches, ownership changes (acquired by Verizon, then Apollo Global Management, rebranded as Yahoo then as Oath then as Yahoo again), and repeated product degradation. The address survived but the platform around it became increasingly hostile to the users it depended on.

**Generation 3: Dominant platform — `rinie@gmail.com`**

Gmail launched in 2004 and is now the dominant global email platform. It is larger, more stable, and better engineered than Hotmail or Yahoo Mail. The coupling is real but the carrier is less likely to disappear.

Except Google has a documented history of killing products. Google Reader (killed 2013). Google+ (killed 2019). Google Inbox (killed 2019). Hangouts (killed 2022). Stadia (killed 2023). Each of these had loyal users who had invested semantically in the platform. Each was shut down on Google's schedule, not the user's.

Gmail is probably safe. `@gmail.com` is probably not going away. But "probably" and "Google's choice" are not the same as owning your own semantic identity.

---

## 3. Enshittification: The Platform Lifecycle

Cory Doctorow coined the term **enshittification** for the predictable lifecycle of platform businesses:

1. The platform is good to users to attract them and grow adoption
2. The platform degrades the user experience to extract value for business customers
3. The platform degrades the experience for business customers to extract maximum value before eventual collapse

Every `@platformname.com` email address is a bet that the platform either never reaches stage 2 or that you will notice and escape before stage 3.

Hotmail went through the full cycle. Yahoo Mail is deep in stage 2. Gmail is somewhere in stage 1 transitioning to stage 2 — the inbox has become more cluttered, the promotions tab buries legitimate email, Google One upsells storage, the interface has been redesigned multiple times for reasons that serve Google's interests more than users' needs.

The enshittification risk is not just that the platform disappears. It is that the platform degrades while your identity remains hostage to it. You cannot leave without paying the migration tax. The degradation is the leverage.

---

## 4a. Google+: When Def-Push Meets a Social Network

Google+ is the cleanest Def-push failure story in tech history — Google had every resource advantage and still lost, precisely because the Def was so confident in itself that it could not hear the Use signal.

Google+ launched in 2011 with a technically superior social graph. **Circles** — a more sophisticated privacy model than Facebook's binary friend/not-friend — was genuinely better designed. Users found it confusing and never used it the way Google intended. The Def was correct in narrow technical terms. The Use signal said otherwise from day one.

**The real names policy** is the sharpest Def-push moment in the story. Google decided that authentic identity meant your legal name. The Use — actual humans who had built online identities around handles and pseudonyms for decades — had a completely different semantic model of identity. LGBTQ users, domestic abuse survivors, activists in repressive countries, professionals with separate personal and work identities: all had legitimate reasons for not using their legal name. Google's Def said they were wrong. The backlash was immediate. Google eventually partially retreated but the damage to trust was permanent.

**The forced YouTube integration** in 2013 is the most visceral example of Def-push at scale. Google replaced YouTube's comment system with Google+ comments, requiring a Google+ account to comment on videos. The Use signal was the loudest in YouTube's history — a petition with millions of signatures, top-rated comments on every video being complaints about the change, Linus Torvalds publicly refusing to comply. Google reversed it in 2015 after two years of sustained user fury. The Def was pushed onto hundreds of millions of users who never asked for a social network and never wanted one.

**The metrics Google watched were vanity metrics.** Account creation and sign-in numbers looked healthy because Google forced the sign-ups through Gmail and YouTube integration. The metrics that mattered — posts, comments, genuine social interaction — were terrible. Classic Gutenberg signals (accounts created, logins recorded) masquerading as Semantic engagement (people actually wanting to be there).

Google had resources, user base, technical talent, and distribution. They lacked the one thing that would have told them the product was failing: a genuine Use-pull feedback loop. The feedback they received they overrode with the real names policy and forced integrations. The Def was so confident in itself that it could not hear the Use signal until the product was already dead. Google+ was shut down in 2019 after a security breach provided the final justification for what the Use signal had been saying for eight years.

---

## 4. `rinie.github.io` and the Own-Domain Question

A personal domain — `rinie.nl`, `rinie.dev`, `rinie.io` — is the correct answer to the email trap. Not because it is easy, but because it separates the semantic layer (your identity, your address) from the Gutenberg carrier (whoever is hosting the email) in the same way number portability separated your phone identity from your network.

`rinie@rinie.nl` pointed at Google Workspace means:
- The semantic identity (`rinie@rinie.nl`) is yours — registered, paid for, controlled by you
- The Gutenberg carrier (Google's servers, Google's spam filtering, Google's interface) is replaceable
- If Google enshittifies beyond tolerance, you change the MX records to Fastmail, or ProtonMail, or self-hosted — in an afternoon, with no address change, with no migration tax

This is exactly the number portability model, self-administered. The phone number equivalent of owning your domain is having a number that any carrier must accept. The difference is that phone portability was legislated. Email portability requires you to engineer it yourself.

`rinie.github.io` raises the same question one level up. The content — the blog posts, the markdown files, the git history — is fully portable. The Gutenberg carrier (GitHub Pages, GitHub's servers, Microsoft's infrastructure) is replaceable. Clone the repo, point a new host at it, update DNS, done.

But `rinie.github.io` is still GitHub's domain. If you care about the address being stable — if you want people to link to posts that survive GitHub's own enshittification — a custom domain (`rinie.nl` pointed at GitHub Pages) completes the separation. The semantic identity (your domain, your URLs) is stable. The Gutenberg carrier (GitHub Pages today, Cloudflare Pages tomorrow if needed) is replaceable.

GitHub has not enshittified meaningfully since Microsoft's acquisition — if anything it has improved. But GitHub is Microsoft, and Microsoft has sunset platforms before. The custom domain is cheap insurance.

---

## 5. The Correct Structure Nobody Uses

The fully separated email setup looks like this:

```
rinie@yourdomain.nl
    ↓ MX records (Gutenberg addressing)
Google Workspace / Fastmail / ProtonMail (Gutenberg carrier)
    ↓ IMAP / webmail (semantic interface)
Your inbox (semantic layer)
```

Your contacts see `rinie@yourdomain.nl`. They always will, regardless of which carrier you are using behind it. When you switch carriers you change the MX records — a DNS entry that takes minutes to update and hours to propagate. Your contacts notice nothing. Your semantic identity is completely stable. The Gutenberg carrier is completely replaceable.

This is how businesses have managed email for decades. `contact@company.com` survives the company moving from Exchange to Google Workspace to Microsoft 365 and back, because the domain is the company's, not the carrier's.

Individual users almost never do this because:
- It requires buying and managing a domain (€10-15 per year, moderate technical setup)
- The benefit is invisible until the carrier enshittifies or disappears
- The migration tax only becomes visible when it is too late to avoid paying it

The ISP email trap and the platform email trap succeed because the cost of the coupling is deferred and the benefit of the separation requires upfront effort. This is the same dynamic as every other coupling in the previous post: the company holding the coupling has every incentive to maintain it, and you only discover the cost when you want to leave.

---

## 6. What Federation Was Supposed to Provide

Email was designed as a federated protocol. Anyone can run an email server. Any domain can send and receive email. The semantic identity (your address) was always intended to be portable between servers — you move to a new server, update the MX record, done.

**The license plate analogy makes the DNS role concrete** — and interestingly, different countries put the boundary in different places.

In the **Netherlands**, the license plate (kenteken) is tied to the car, not the owner. The plate is a Gutenberg identifier — it identifies the physical vehicle. When you sell the car the plate goes with it. When you buy a new car you get a new plate. Your identity as owner lives in the registration document (kentekenbewijs), which is separate. The RDW database is the DNS: it resolves the plate (Gutenberg address) to the current owner (semantic identity).

In the **UK, Belgium, and many other countries**, the plate can be transferred to a new vehicle. It travels with the owner, not the car. The plate becomes a semantic identifier — your identity as a registered driver — separated from the Gutenberg carrier (the specific car). In the UK you can buy and sell personalised plate numbers independently of any car. Pure semantic identity trading, with the DVLA database as the DNS resolver mapping the plate to whichever car currently carries it.

Both systems work. They just put the Gutenberg/Semantic boundary in different places:
- **Dutch system**: plate = Gutenberg (identifies the vehicle). Ownership tracked separately in the database.
- **Owner-portable system**: plate = Semantic (identifies the owner). The car is the replaceable Gutenberg carrier.

The RDW and DVLA databases are the resolvers in both cases — the DNS that makes either model function. The plate is only meaningful because the database maps it to something. Without the resolver, it is just a sequence of characters on a piece of metal.

Email's `@domain.nl` is the plate. The MX record is the resolver. The email server is the car. In the platform email model, the platform owns both the plate and the resolver — you have no more control over your address than a Dutch car owner has over their plate when they sell the vehicle. In the own-domain model, you own the plate and control the resolver. The car (the email provider) is replaceable. The plate (your address) is yours.

The federation is still there technically. What collapsed was the user practice of owning the domain in their address. When users accepted `@hotmail.com` and `@gmail.com` addresses they voluntarily gave up the separation that the protocol provided. The technical capability for portability was always present. The social and economic incentives pushed in the opposite direction.

This is the same story as the open web versus walled gardens. The web is federated — anyone can run a server, anyone can publish a URL. Social media platforms offered convenience and network effects in exchange for hosting your identity on their domain. The semantic content (your posts, your connections, your identity) moved inside the carrier. When the carrier enshittifies, the content is hostage.

ActivityPub and the Fediverse are the attempt to rebuild federation at the social layer — `rinie@mastodon.social` or better `rinie@yourdomain.nl` via a self-hosted instance. The same separation: own the domain, make the carrier replaceable. The adoption curve is the same as own-domain email: correct in principle, inconvenient in practice, invisible benefit until you need it.

---

## 7. The Checklist Revisited

In the previous post the closing question was: does my semantic investment travel freely, or is it coupled to this carrier?

For email specifically:

| Address type | Who owns the domain | Portability | Risk |
|---|---|---|---|
| `you@isp.nl` | Your ISP | None | ISP closes, acquired, or you switch |
| `you@hotmail.com` | Microsoft | None | Platform enshittifies or closes |
| `you@gmail.com` | Google | None | Platform enshittifies or closes |
| `you@yourdomain.nl` via Google | You | Full — change MX records | Carrier enshittifies, you move in hours |
| `you@yourdomain.nl` self-hosted | You | Full | Technical burden on you |

The domain registration is the number portability. The MX record is the SIM. The email provider is the carrier. When you own the domain, switching carrier is changing one DNS record. When you don't, switching carrier means rebuilding your identity from scratch.

---

## 8. The Practical Step

If you have an important email address at a platform you do not own, the migration is not immediate — you cannot change the address your contacts have for you overnight. But you can start the separation today:

1. Register a domain you own (`yourdomain.nl`, about €10-15/year)
2. Set up email at that domain via Google Workspace, Fastmail, or ProtonMail
3. Start giving the new address for new contacts and services
4. Gradually migrate existing services to the new address when you interact with them
5. Keep the old address working as a redirect or backup while the transition completes

The migration takes months not days — the same as any address change. But at the end of it your semantic identity is yours. No platform can take it. No acquisition can change it. No enshittification can hold it hostage.

Your music survived six formats because the semantic content eventually became carrier-free. Your phone number survived multiple phones and providers because regulation forced the separation. Your email address can survive any platform — but only if you make the separation yourself, because nobody is going to legislate it for you.

Own the domain. Make the carrier replaceable. The box should not outlive the thing inside it. Make sure it doesn't.
