---
layout: post
title: "Embracing the Web Instead of Fighting It"
date: 2026-07-13
tags: [gutenberg-semantic, web, markdown, nodejs, typescript, electron, vscode, xml]
level: general
description: "XML fought the web's clean seams and lost. Markdown embraced them and won. Node.js, TypeScript, Electron, and VS Code are the story of what happened when the development ecosystem stopped demanding a wider track and started building on the shelf that was already there."
---

The [previous post on web craft](https://rinie.github.io/2026/06/06/working-on-the-same-page/) described the web's clean seams: HTML for structure, CSS for presentation, JavaScript for behaviour, HTTP for transport. Four layers, each independent, each replaceable. The seams were clean from the start.

This post is about what happened next. After Berners-Lee planted the seed and stepped back. After the ecosystem had to decide: work with the web's model, or fight it.

Some fought it. Some embraced it. The ones that embraced it are still here.

---

## XML: Fighting the Web

XML arrived in 1998 with a promise: one format to rule all data. Self-describing, extensible, rigorous. Every element carries its own namespace. Every document declares its own schema. The structure and the content travel together, inseparable, the envelope stapled to every page of the letter.

The signal-to-noise ratio was poor from the start. Compare:

```markdown
## The Postman Reads the Envelope
```

with:

```xml
<section xmlns="http://docbook.org/ns/docbook" version="5.0">
  <title>The Postman Reads the Envelope</title>
</section>
```

Same content. The Markdown is the content. The XML is the content wrapped in self-description that repeats on every element, in every file, at every level of nesting.

XML demanded a wider track. Not HTML's track — a new track, with new parsers, new tooling, new everything. SOAP, WSDL, DocBook, RSS 1.0, Atom — all of them XML, all of them carrying the envelope inside the letter, all of them requiring the postman to read the manifest before touching the package.

The web ignored XML for most purposes. HTML survived. JSON replaced XML for data exchange — not because JSON is more correct, but because JSON fits in a JavaScript variable without a parser. The track was already there. JSON stayed on it. XML demanded a new one.

---

## Markdown: Embracing the Content Layer

Markdown arrived in 2004. John Gruber's insight was simple: write the way you already write in plain text email, and the structure will be obvious. `#` for headings. `**` for bold. `-` for lists. The content is above the waterline. The HTML is what the renderer produces below it.

The signal-to-noise ratio is excellent. The diff is the content. When a paragraph changes, the diff shows the changed words — not the changed tags, not the changed attributes, not the changed namespace declarations. The content and only the content.

**Markdown made HTML diffable.**

The git history of a Markdown file is the history of the ideas. The git history of an HTML file is the history of the markup mixed with the history of the ideas. The seam between content and presentation stays clean in Markdown because Markdown keeps them in separate systems — the content in the `.md` file, the presentation in the CSS, the rendering in the browser or the static site generator.

This is why the series is in Markdown. Sixty posts, one git repo, every change diffable. The posts are the content. Jekyll is the renderer. The Billy holds the Markdown. The CDN serves the HTML. None of these layers knows about the others' internals.

---

## Node.js: The Same Language Across the Seam

In 2009 Ryan Dahl looked at the seam between the browser and the server and made a simple observation: the seam required learning two different languages. JavaScript on the client. Python, Ruby, Java, PHP on the server. The HTTP request crossed a language boundary on every round trip.

Node.js removed the language boundary without removing the seam. JavaScript on the server. The same language above and below the HTTP layer. Not because JavaScript is the ideal server language — because removing one unnecessary seam reduces friction without requiring anyone to change the seam that matters (the HTTP protocol, the JSON format, the URL structure).

The web stayed the web. The server joined it instead of opposing it. The 10x that followed — npm, the package ecosystem, the explosion of JavaScript tooling — was possible because the language boundary had been removed and the remaining seams were clean.

---

## TypeScript: Types Above the Waterline

TypeScript arrived in 2012. Microsoft's insight: add types to JavaScript without changing the JavaScript that runs in the browser.

The type annotations are the Semantic layer — the meaning, the contracts, the guarantees. The compiled JavaScript is the Gutenberg layer — the bytes the browser actually executes. TypeScript strips the type annotations before deployment. The browser sees plain JavaScript. The developer sees typed code.

The track never widened. The browser did not need updating. Every existing JavaScript file is valid TypeScript. The old trees kept standing while the new ones grew with type safety.

Compare with alternatives that fought the web: CoffeeScript, Dart, ClojureScript — all of them compiled to JavaScript but were not JavaScript. The seam between "what you write" and "what runs" was wide and unfamiliar. TypeScript made the seam invisible. What you write is almost exactly what runs, with the type information removed.

Markdown is to HTML as TypeScript is to JavaScript. The Semantic layer (the meaning, the structure, the types) above the waterline. The Gutenberg layer (the HTML, the JavaScript) unchanged below it.

---

## Electron: The Desktop Became a Web Page

In 2013 GitHub built Atom using web technologies — HTML, CSS, JavaScript — running in a desktop application shell. The insight: the browser already solves the hard problems of text rendering, layout, event handling, and accessibility. Why solve them again for the desktop?

Electron wraps a Chromium browser in a desktop application frame. Your application is a web page. The same HTML, CSS, and JavaScript that run in the browser run in the desktop application. The seam between web and desktop disappeared — not by making the web more like the desktop, but by making the desktop more like the web.

VS Code is built on Electron. The editor you are reading this in (if you use VS Code) is a web page running in a Chrome frame. The syntax highlighting is CSS. The file tree is HTML. The extension API is JavaScript. The same technologies that render `rinie.github.io` render your code editor.

The extension ecosystem followed naturally. Any JavaScript developer can write a VS Code extension. The Billy that holds web extensions also holds editor extensions. Same shelf. Same dimensions. No new track required.

---

## The Stack That Embraced the Web

The result of thirty years of embracing rather than fighting:

- **Markdown** — content above the waterline, rendering below it, the diff shows the ideas
- **Node.js** — JavaScript across the HTTP seam, the language boundary removed
- **TypeScript** — types in the Semantic layer, JavaScript in the Gutenberg layer, the track unchanged
- **Electron** — the desktop IS the web, one stack everywhere
- **VS Code** — the editor is a web page, extensions are JavaScript, the ecosystem inherited

One shelf. Everything fits. The web is the Billy. The frameworks that embraced it are still running. The frameworks that fought it are in a pile on the floor.

---

## The Architect No Longer Leads. The Web Stayed. 10x Use.

Berners-Lee designed the seams. The web outgrew his direct stewardship — not because he failed, but because the 10x arrived. Millions of developers, billions of pages, an ecosystem too large for any architect to lead. The web became what Clay Shirky described: when the cost of coordination falls below a certain threshold, entirely new kinds of things become possible. The architect steps back not by choice but by arithmetic. The 10x cannot be led. It can only be learned by going to the Gemba and watching how the seams evolve.

The ecosystem had to decide whether to honour the seams or fight them. Nobody coordinated that decision. It emerged from practice — from millions of developers choosing the path of least resistance, which happened to be the path that the clean seams had already laid.

XML fought them — put the envelope inside the letter, demand a new parser, require a wider track. It lost most of its battles not because anyone decided XML was wrong, but because JSON stayed on the existing track and the 10x of JavaScript developers found JSON easier. Use-Pull at ecosystem scale. Not a committee decision. A billion small choices.

Markdown, Node.js, TypeScript, Electron — all of them honoured the seams. Not because they were told to. Because the seams were already there, already working, already carrying half the internet. The path of least resistance was to stay on the track that was already laid. The 10x use of each one was not planned. It emerged from the same conditions: embrace what is already everywhere, stay on the existing track, let the Gutenberg layer improve underneath you.

The web kept improving underneath them. HTTP/2, HTTP/3, V8 getting faster, the browser as the universal runtime. Each improvement flowed up for free. The Gutenberg layer improved. The Semantic layer collected the gains. The 10x that nobody planned kept arriving because the seams stayed clean.

Practice makes the seams. The seams outlast the architects. The web stayed. The 10x use proved the seams were right.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). The companion post [Working on the Same Page](https://rinie.github.io/2026/06/06/working-on-the-same-page/) covers the web's clean seams as they were designed. Related: [If You Can't Diff It, It's a Dead End Street](https://rinie.github.io/2026/07/08/if-you-cant-diff-it/) on Markdown and the diffable content layer, [Frameworks Move the Seam](https://rinie.github.io/2026/07/06/frameworks-move-the-seam/) on the cost of demanding a wider track, and [The Watershed](https://rinie.github.io/2026/07/02/the-watershed/) on the Linux thread that runs through all of it.*
