---
layout: post
title: "Working on the Same Page"
date: 2026-06-06
tags: [def-use, web, css, javascript, html, craft-boundaries]
level: technical
description: "The web stack spent thirty years building clean Def-Use seams between HTML, CSS, JS, and HTTP. ES modules, CSS custom properties, and OpenAPI are what made specialist crafts possible. CSS-in-JS was a regression."
---

The fantasy of the "full-stack unicorn" is really a fantasy about eliminating seams. If one person masters HTML, CSS, JavaScript, HTTP, UX, accessibility, and performance simultaneously, nobody has to negotiate the boundaries between those crafts. The price is that you need a Leonardo da Vinci on every team — or you accept that every layer leaks into every other layer and everyone is permanently blocked on everyone else.

The web stack has been on a thirty-year arc of doing the opposite: making the seams *cleaner*, which is what allows specialists to go deeper rather than wider. This post is about that arc and why it matters more than any individual framework or tooling trend.

---

## 1. The Def-Use Split as a Craft Boundary

The [Def-Use model](https://rinie.github.io/2026/05/16/def-use-lean-pull/) describes every layer in terms of who publishes a contract and who consumes it. The key insight is that every layer in the web stack defines a Def-Use seam where different crafts can hand off cleanly.

| Layer | Def (publisher) | Use (consumer) | Craft boundary |
|---|---|---|---|
| **HTTP** | Backend / infra engineer | Browser, CDN, JS fetcher | Headers, status codes, caching |
| **HTML** | Semantic / content author | Browser, screen reader, crawler | Structure without presentation |
| **CSS** | Design systems engineer | HTML author, component author | Visual contract, not logic |
| **JS** | Application engineer | User interaction, data binding | Behaviour, not structure |

The failure mode — old PHP, jQuery spaghetti, inline styles everywhere — is when these layers bleed into each other. When CSS lives in JS template strings. When HTML carries presentation meaning through class names chosen by the person who wrote the CSS that week. When cache semantics are baked into fetch logic inside application code. That forces everyone to be Leonardo, or it forces one person to hold the whole picture because no interface is stable enough for a specialist to stand on.

---

## 2. HTML5 Semantics: Structure Becomes Def

Before HTML5, structure was expressed through class names: `.sidebar`, `.hero`, `.nav-item`. Class names are owned by whoever wrote the CSS. When the HTML author and the CSS author are different people — or the same person at different times — that's an invisible coupling. The HTML cannot be read without the CSS.

HTML5 moved structural meaning into the document itself. `<article>`, `<nav>`, `<main>`, `<figure>`, `<time>` — these are Def statements in the document layer. A screen reader, a search crawler, and an accessibility engineer can all Use them without touching the CSS. The content author and the design engineer now work in genuinely separate layers, with HTML as the published contract between them.

This is not just semantic nicety. It is what makes a design system possible. If your HTML expresses structure, a CSS engineer can redesign the entire visual layer without asking the content team to change a single character. The Def is stable; the Use can vary.

---

## 3. CSS Custom Properties and Cascade Layers: Def Inside Def

CSS3 and modern CSS gave CSS a Def layer *of its own*. Design tokens live at `:root` — defined once by the design system team, consumed everywhere by component authors who never need to know the hex value or the spacing unit. Change the token, the whole system updates. The component author is a Use-side consumer of a contract they did not write and do not maintain.

`@layer` (now widely supported) went further. Framework CSS and application CSS can coexist with explicit, declared precedence. A UI library author and an application developer are no longer in a specificity war, fighting over who wrote their selector last and who remembered to add `!important`. The layer *is* the Def; the cascade is the Use. Different crafts, same file, no collision.

---

## 4. ES Modules: JS Gets a Real Def-Use Boundary

This is probably the single most important change for craft separation in the frontend. Before ES modules, JavaScript had one global scope. Every author had to know what everyone else had defined. Libraries attached themselves to `window`. Scripts had to load in the right order. Nothing could be private because there was no concept of private.

`export` is a Def statement. `import` is a Use statement. They are explicit, file-scoped, and machine-readable. A component author exposes a deliberate API surface and hides the implementation. A page author imports and uses it without reading the source. This is the moment JavaScript acquired the ability to support real craft separation — a library author and an application author working on the same codebase without needing to hold each other's mental model.

Web Components push this to the DOM level. A UI engineer defines `<date-picker>` as a black box with a clean attribute API. A template author uses it with zero knowledge of its internals. The HTML attribute interface is the Def; everything inside the shadow DOM is the Use-side's private concern.

---

## 5. HTTP Headers: The Infra Craft Gets Its Layer Back

If cache semantics are embedded in application code — `if (url.includes('user')) { skipCache = true }` — then the infrastructure engineer cannot own caching strategy. They have to read every JS file to understand what the application is actually doing to the cache. The Def and Use are tangled.

`Cache-Control`, `ETag`, `Vary`, `CDN-Cache-Control` — these are all Def statements at the HTTP layer. The backend publishes freshness semantics; the browser, the CDN, and the JS fetcher consume them according to the protocol. A platform engineer can own caching strategy, CDN configuration, and edge behaviour entirely within the HTTP layer, without touching application code. HTTP/2 and HTTP/3 went further: multiplexing, prioritisation, and server push are all protocol-level concerns that application code does not have to manage.

The infra craft is only 10x when it has a layer it actually owns.

---

## 6. OpenAPI: The HTTP Contract as a First-Class Def

The final seam — between backend and frontend — hardened with machine-readable API contracts. OpenAPI does not describe the implementation; it describes the contract. The backend engineer publishes a schema; the frontend engineer generates types from it and works against the contract, not the implementation. Neither side reads the other's code. The HTTP call is a Use; the OpenAPI spec is the Def.

This is what makes a 10x backend engineer possible on a team with a 10x frontend engineer. They can work in parallel, validate independently, and deploy separately, because the contract is explicit, versioned, and not embedded in anyone's head.

---

## 7. The Remaining Friction Points

Not all the seams are clean yet.

**CSS-in-JS** (styled-components, emotion) was a regression. It collapsed the HTML/CSS/JS boundary in the name of "componentization" and forced the design engineer to work inside the application engineer's file. The pendulum is swinging back — CSS Modules, vanilla-extract, CSS Custom Properties — but the episode showed how quickly a Def-Use seam can be re-tangled when the tooling makes tangling convenient.

**SSR and hydration** blur the HTTP/JS boundary again. What executes on the server and what executes in the browser becomes a framework concern, not a protocol concern. Next.js and Nuxt are powerful, but they require someone to hold the full picture of the execution boundary — which is exactly the thing craft separation is supposed to avoid.

**Build tooling** — Webpack, Vite, esbuild, Rollup — is a no-man's-land that sits across every layer simultaneously. Everyone depends on it, few master it, and when it breaks, the craftsperson who can diagnose it is whoever happened to configure it.

These are exactly the places where you still need a boundary owner. Not Leonardo the painter, but an engineer whose *craft is the seam itself* — someone who knows both sides well enough to hold the interface stable while specialists on either side go deep.

---

## 8. Why 10x Best-of-Breed Beats Leonardo

A 10x CSS engineer who deeply understands the cascade, layout algorithms, custom properties, and animation can deliver something a generalist never could. But only if they do not have to understand the JS state machine to do it. A 10x infra engineer can make a system fast, resilient, and cheap to operate — but only if cache semantics are not buried in application logic.

The Leonardo trap is a seam-pollution problem. When layers bleed, you need one person to hold the whole picture, because no interface is stable enough for a specialist to stand on. Clean seams are not a nice architectural principle. They are the prerequisite for hiring specialists instead of generalists, for parallel work instead of sequential work, for the kind of team where everyone is working on the same page — just different parts of it.

The web stack spent thirty years building those seams. They are worth protecting.

---

## 9. Evolution Beats the Static Expert

There is a final argument for clean seams that goes beyond team coordination: **no one can predict what the web stack looks like in five years.**

The full-stack unicorn who mastered React, Redux, Webpack, REST, and SQL in 2019 spent five years optimising for a stack that is now being replaced layer by layer. Nobody predicted:

- CSS container queries making component-level responsive design a browser primitive
- View transitions making page navigation animations part of the platform, not the framework
- Islands architecture as a serious alternative to full SSR or full SPA
- Web Components finally becoming viable after a decade of false starts
- AI-assisted generation changing what writing a component even means
- HTTP/3 and edge computing changing the performance model of every HTTP request

The specialist who went deep on CSS layout algorithms collected the container query dividend for free — no rewrite, just new capability on a layer they already owned. The specialist who owned the HTTP layer collected HTTP/3 and edge caching improvements for free. The specialist who kept JavaScript at clean ES module boundaries collected every bundler and tree-shaking improvement for free.

The unicorn who collapsed the seams owns the debt of every future evolution across every layer simultaneously. When CSS gets a new capability, they have to unpick the CSS-in-JS. When HTTP gets a new caching primitive, they have to find it buried in application logic. When the JS module system improves, they have to untangle the global scope they never bothered to clean up.

This is the [Moore's Law architectural principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) applied to the web stack. Accept the 10% coordination overhead of maintaining clean seams. Collect the 2x improvement from each layer's evolution for free. The generalist who eliminated the seams cannot benefit from improvements to layers they collapsed. The specialist can — because the seam is exactly where the future improvement enters the system.

Evolution beats the static expert not because the expert is incompetent but because no single mind can predict which layer improves next, in which direction, on which timeline. The arrogance of the full-stack unicorn is not technical — it is temporal. They are optimising for a known present at the cost of an unknown future.

The clean seam is the option that stays open. The collapsed stack is the option exercised too early. The web will change. The question is whether your architecture changes with it or fights it.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Def-Use, Lean Pull, and Why the User Does Not Sux](https://rinie.github.io/2026/05/16/def-use-lean-pull/) on the Def-Use split as a feedback model, and [The Boundary Has a Lifecycle](https://rinie.github.io/2026/05/26/boundary-lifecycle/) on how clean boundaries form, drift, and break over time.*
