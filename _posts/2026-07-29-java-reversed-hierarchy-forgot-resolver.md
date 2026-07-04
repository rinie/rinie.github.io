---
layout: post
title: "com.google.gson Goes Nowhere: Hardcoded Names Are as Bad as Hardcoded Numbers"
date: 2026-07-29
tags: [gutenberg-semantic, java, dotnet, npm, cargo, resolver, dependency, modularity, es6, import]
level: technical
description: "Java and .NET reversed DNS hierarchy to guarantee unique names, then stopped — no resolver. A hardcoded package name is structurally identical to a hardcoded IP address: both skip the resolver and bind the Use to the Def's iceberg address. Modularity means the Use can replace the Def by another implementation. Without a resolver, that is not possible."
---

DNS resolves `gmail.com` to an IP address. Nobody hardcodes `142.250.185.5` into their application — because the IP might change, the load balancer might shift, the CDN might reroute. The name is stable. The location is not. The resolver keeps them apart.

Java package names look like they apply the same principle:

```
DNS:    gmail.com          → resolver → 142.250.x.x
Java:   com.google.gson    → nothing  → ???
```

The hierarchy is borrowed from DNS, reversed, and then the design stops. No resolver. No network lookup. No mapping from name to location. `com.google.gson` is `142.250.185.5` with dots rearranged — a Gutenberg fact embedded where a resolver should be.

Hardcoded names are as bad as hardcoded numbers. Both skip the resolver.

---

## The Name Clings to the Iceberg

`import com.google.gson.Gson` does not name a class. It names an address in an iceberg.

The package hierarchy encodes the Def's internal organisation — `com.google.gson` is not a semantic description of what the code does, it is the Def's choice of namespace, vendor prefix, and project name baked into the import statement. The Use side has no lever. The Def wrote its iceberg address into every file that consumes it.

This matters when you want to swap implementations. Gson and Jackson both parse JSON. If you decide to replace Gson with Jackson, every file containing `import com.google.gson.Gson` has to be found and changed. The Def's address is in the Use's code. The Use cannot replace the Def without knowing where it lives.

That is not modularity. Modularity means the Use can replace the Def by another implementation without the Def knowing, and without the Use's code knowing the Def's internal address.

Java `import` fails this test. The iceberg address is load-bearing.

.NET does the same:

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
```

`System.Collections.Generic` is a Microsoft iceberg address. `Newtonsoft.Json` is a third-party iceberg address. Neither can be remapped. The Use side cannot substitute a different implementation without touching every `using` directive that names the old one.

---

## Global Pollution: When the Def Decides What Enters Your Namespace

`import *` is the logical endpoint of Def-Push naming. The Def decides which names enter the Use's namespace. The Use has no control over what arrives or whether it collides with something already there. The namespace belongs to whoever wrote the import.

Python's `from json import *` and Java's `import com.google.gson.*` both hand the namespace to the Def. The Use side loses the ability to see where a name came from or remap it to something else. Two libraries exporting the same name silently shadow each other. The resolver is not just missing — the Use's ability to manage its own namespace is gone.

---

## ES6 Import Maps: Use-Pull Naming

ES6 modules separate the name from the location by design:

```javascript
import { parse } from 'acorn';
```

`'acorn'` is a semantic identifier — a short name that the Use side chose. It is not an iceberg address. The resolver (the import map, the bundler, the CDN) maps `'acorn'` to whatever physical location is appropriate for this deployment:

```json
{
  "imports": {
    "acorn": "https://cdn.example.com/acorn@8.11.3/acorn.js"
  }
}
```

The Use can remap `'acorn'` to a different version, a local path, a polyfill, or a different implementation entirely — without touching any file that uses it. The Def's code does not change. The Use's import statements do not change. Only the mapping changes.

That is Use-Pull: the consumer controls the resolution. The Def publishes under a short semantic name. The Use side decides where that name resolves.

npm and cargo extend this to the package level:

```
npm install acorn      → registry.npmjs.org → specific tarball → ✓
cargo add serde        → crates.io          → verified download → ✓
```

The name is short and semantic. The resolver finds the physical artifact. The Use can pin a different version, fork the package, or substitute a compatible implementation — all by changing the mapping, not the code.

---

## Modularity Is Replaceability

The test for real modularity is not whether code is organised into packages. It is whether the Use can replace the Def with a different implementation without touching the Use's own code.

```
Real module:   Use says 'acorn', import map resolves it, swap the map to change the Def   ✓
Java import:   Use says com.google.gson.Gson, change Def means change every Use file      ✗
DNS:           Use says gmail.com, DNS resolves it, swap the A record to change the Def   ✓
Hardcoded IP:  Use says 142.250.185.5, change Def means change every Use file             ✗
```

The pattern is the same at every layer. When the Def's address is embedded in the Use's code, the Use is not consuming a module — it is bound to an iceberg. The name might be long and hierarchically organised, but it functions as a magic number: a specific physical fact that breaks when the thing it points at moves.

Java gave packages globally unique names and called it modularity. What it gave was a namespace with no resolver — a filing convention, not an address system. Maven Central arrived thirty years later to add what should have been there in 1995: a real resolver that maps the name to a location.

The hierarchy was borrowed from DNS. The lesson of DNS — that the name and the location must be kept separate by a resolver the Use side controls — was not.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [URL, DNS and the @ Sign](https://rinie.github.io/2026/07/28/at-sign-layer-boundary/) on email and the web keeping the global layer small and externally resolvable, [UUIDs Are Not Names](https://rinie.github.io/2026/05/27/uuids-are-not-names/) on identifiers that name without locating, and [Your Email Address Is Hostage](https://rinie.github.io/2026/05/25/email-address-hostage/) on owning the resolver so the carrier stays replaceable.*
