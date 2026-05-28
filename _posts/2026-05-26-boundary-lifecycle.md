---
layout: post
title: "The Boundary Has a Lifecycle: From Unix Portability to WebAssembly"
date: 2026-05-26
category: "Software Architecture"
tags: [architecture, boundaries, unix, wasm, evolution, portability]
depth: technical
---

This is part of a series. The [Gutenberg/Semantic model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) established that the best systems isolate the physical and logical layers in one place and keep the boundary clean. This post asks a harder question: what happens to the boundary over time? How does it form, stabilise, drift, break, and reset — and what strategies have worked for managing it?

---

## 1. The Newer Machine Test

The cleanest test of boundary quality is the newer machine upgrade. Take your code. Move it to faster hardware with more memory and an SSD. Do not change a line.

If the code just runs faster — the boundary is clean. The Gutenberg layer (hardware, OS, libc) improved underneath. The semantic layer (your code) harvested the gain for free. This is Moore's Law as a free upgrade: faster CPU, more RAM, lower I/O latency, all delivered without touching the semantic contract above.

If you need to retune constants, relink against a new libc version, update generated stubs, or recompile for a new ABI — Gutenberg details have leaked into the semantic layer somewhere. The boundary was not as clean as it looked.

Well-designed code has no Gutenberg constants embedded in it. Buffer sizes come from config or are auto-detected. Thread pool sizes are derived from core count at runtime. Memory allocations are sized by the allocator, not by a hardcoded number that made sense on the old machine. The Gutenberg layer improves and the semantic layer harvests the gain transparently — not because the developer was lucky, but because they kept the boundary clean.

---

## 2. The Boundary Lifecycle

Gutenberg/Semantic boundaries do not spring into existence fully formed. They go through a lifecycle:

**Informal** — the boundary exists but is not specified. Early Unix: the same C source compiled on different hardware because the C language and syscall interface happened to be consistent, not because anyone had formally defined a portability contract. Code worked but depended on undocumented behaviour. Portability was possible but fragile.

**Formalised** — the boundary is specified, versioned, and explicitly maintained. POSIX formalised the Unix syscall interface. iOS ABI stability formalised the Objective-C and Swift framework boundaries. Swift ABI stability (iOS 13) was a Rosetta-style moment: before it, every app bundled its own Swift runtime, coupling the semantic layer to a specific Swift version. After it, Swift became a stable platform ABI and apps could rely on the system runtime. Breaking changes are now announced, managed, and rare.

**Managed** — the boundary implementation is actively maintained to deliver improvements without breaking the contract above. Security patches arrive. Performance improves. New hardware is supported. The semantic layer above notices nothing. Android Mainline, Windows WinSxS, containers, and package manager lock files are all managed boundary strategies — different mechanisms, same goal.

**Broken** — the boundary is nominally present but in practice either too permeable (Gutenberg details leak into the semantic layer) or too rigid (the semantic layer is coupled to a specific Gutenberg implementation). DLL Hell, gRPC's IDL coupling, YAML's significant whitespace. The boundary exists on paper. It does not hold in practice.

**Security-forced reset** — the Gutenberg implementation is compromised. The boundary must be redrawn even at the cost of breaking the semantic layer above. OpenSSL after Heartbleed. Log4Shell. These are the legitimate breaking changes — necessary, painful, and the exception that proves the rule.

---

## 3. Unix: Informal to Formalised

Unix made running the same program on a different computer achievable for the first time at scale. The mechanism was a thin syscall interface — a small set of operations (open, read, write, fork, exec) that any Unix implementation had to support. Code written to the syscall interface would compile and run on any Unix machine, regardless of the underlying hardware.

This was informal portability. The syscall interface was consistent by convention and by the cultural inheritance of Bell Labs Unix, not by formal specification. When vendors diverged — BSD versus System V, each adding their own syscalls and behaviours — the informal boundary fractured. The Unix wars of the 1980s were fundamentally a boundary dispute: whose informal contract was the real one?

**POSIX** (1988) formalised the boundary. The Portable Operating System Interface defined exactly which syscalls, which library functions, and which behaviours any conformant Unix had to provide. Code written to POSIX was genuinely portable — not as a convention but as a contract. Linux, macOS, FreeBSD, Solaris: all POSIX-conformant, all able to run the same source code.

POSIX is also where `fread()` gets its formal definition. The boundary between the bytestream (Gutenberg) and the characters (Semantic) is specified precisely enough that any conformant libc must implement it consistently. The informal portability of early Unix became the formal contract of POSIX — the boundary moved from cultural to contractual.

---

## 4. libc: The Implementor of the Boundary

libc sits exactly at the Gutenberg/Semantic boundary. It is not just an interface — it is the component that *implements* the boundary. When libc changes, userspace code that assumed a specific boundary position can break without changing a line.

**glibc versus musl** is the live version of this debate:

**glibc** — the GNU C Library, the de facto standard on Linux. Rich, widely compatible, dynamically linked by default. When glibc improves (better allocator, better threading, better buffering) your binary gets the improvement for free at runtime — as long as the ABI is stable. The risk: glibc occasionally shifts behaviour in ways that break code relying on unspecified or undefined behaviour. Code that was technically wrong but practically working stops working when glibc becomes more conformant.

**musl** — minimal, predictable, static-linking-friendly. Trades the free-upgrade benefit of glibc improvements for a more stable, auditable boundary. A musl-linked binary behaves identically on any system because it carries the boundary implementation with it. The Gutenberg layer cannot shift under it because the Gutenberg layer is bundled inside.

**Containers** resolve this tradeoff pragmatically: bundle a specific libc version with the application. The boundary is frozen at container build time. You trade the free-upgrade benefit for reproducibility and explicit control. When you want the upgrade, you rebuild the container — a deliberate boundary update, not an implicit one.

This is the same tradeoff as npm's `^4` versus `4.0.0` exact. Dynamic linking with glibc is `^` — you get improvements automatically. Static linking with musl is exact pinning — you get stability and predictability. The question is whether you trust the boundary to be stable enough to benefit from dynamic linking's free upgrades without being hurt by its boundary shifts.

---

## 5. Android: Progressive Boundary Formalisation

Android started with an informal boundary. OEMs and carriers sat between Google and the user, free to modify the Android framework, delay updates, add bloatware, and make the Gutenberg/Semantic boundary whatever they chose. A security patch from Google might take eighteen months to reach a device — if it ever arrived. The Def-push of OEMs and carriers was structurally enabled by the absence of a formalised boundary.

Google spent a decade progressively formalising the boundary, layer by layer:

**Google Play Services** — a semantic layer that updates independently of the OS, delivered directly by Google, bypassing OEMs and carriers entirely. Location services, authentication, device management: all moved above the OEM/carrier boundary. The carrier could delay an Android OS update; they could not delay a Play Services update.

**Project Treble** (Android 8, 2017) — an explicit formal boundary between the Android framework and hardware vendor code (HAL, Hardware Abstraction Layer). Before Treble, an Android OS update required new HAL code from the chip vendor, creating a dependency chain that delayed or prevented updates. After Treble, the Android framework and the HAL communicate through a versioned interface — the boundary is formal and independently updatable on each side.

**Project Mainline** (Android 10, 2019) — individual OS modules (DNS resolver, media codecs, permission controller, security components) delivered as updates through the Play Store, bypassing the OS update cycle entirely. A security vulnerability in the DNS resolver gets patched in hours across all modern Android devices, not in eighteen months after passing through OEM and carrier update chains.

Each project is a boundary formalisation event: take an informal coupling, make it explicit, version it, and make each side independently updatable. The Def-push of carriers and OEMs was not eliminated — they still control much of the Android experience — but it was progressively contained to a smaller surface area.

---

## 6. iOS: Boundary by Design

Apple's approach to boundary management is the opposite of Android's: tight control by design from the start, rather than progressive formalisation after the fact.

Apple controls the full stack — hardware (Apple Silicon), OS (XNU kernel), libc (based on FreeBSD's), frameworks (UIKit, SwiftUI), distribution (App Store). This gives them the tightest possible boundary management:

**ABI stability** — Objective-C ABI has been stable for decades. Apps compiled years ago still run on current iOS without modification. The Gutenberg layer (the instruction set, the OS ABI, the framework binaries) improves continuously. The semantic contract above is stable.

**Swift ABI stability** (iOS 13, 2019) — before this, every Swift app had to bundle its own Swift runtime, adding megabytes to every download and coupling each app to a specific Swift version. After, Swift became a stable platform ABI. Apps could rely on the system runtime. New apps became smaller; old apps kept working. A boundary formalisation event analogous to Android's Mainline.

**App Store review as boundary enforcement** — Apple rejects apps that use private APIs or make Gutenberg assumptions about hardware specifics. This is not purely about quality — it is boundary management. Apps that bypass the formal interface would break when Apple changes the Gutenberg layer underneath. The review process enforces the semantic contract.

**Bitcode** (deprecated in Xcode 14) — Apple could recompile your app for new hardware architectures without you resubmitting. The Gutenberg layer (the instruction set) was abstracted above the app binary. An app submitted once could run on ARM variants that didn't exist when it was compiled. This was Rosetta generalised: not just a translation layer but a deferred compilation model. Apple deprecated it when Apple Silicon made the ARM family stable enough that per-device compilation was no longer needed.

The cost of Apple's model: Apple owns the boundary entirely. They decide when it moves, how it moves, and whether your app survives the transition. The carrier/OEM Def-push problem does not exist on iOS. The Apple Def-push problem does — but it is a single, predictable, well-resourced Def rather than a fragmented, inconsistent one.

---

## 7. Windows DLL Hell: The Broken Boundary

Windows DLL Hell is the canonical broken boundary story. The mechanism was sound in principle: shared DLLs let multiple applications use the same library code, saving disk space and memory. The boundary was the DLL API — a versioned interface between the application (semantic layer) and the library implementation (Gutenberg layer).

In practice the boundary did not hold. Installers overwrote shared DLLs with different versions. Applications assumed specific DLL versions and broke when an unrelated installer changed them. The semantic contract (the DLL API) was nominally stable but the Gutenberg implementation kept shifting under applications without their knowledge or consent. An application could work perfectly until you installed an unrelated piece of software, at which point it broke silently.

**Windows Side-by-Side (WinSxS)** was the fix: each application gets its own copy of its dependencies, frozen at the version it was tested against. The boundary was stabilised by moving from shared Gutenberg to per-application Gutenberg — essentially static linking implemented at the OS level. The fix worked but created the WinSxS bloat problem: the Windows folder filled with dozens of versions of the same DLLs, one copy per application. Stability at the cost of disk space and complexity.

The lesson: **a shared Gutenberg layer requires a formally maintained boundary contract.** Without it, applications make informal assumptions about the Gutenberg implementation, and those assumptions break when the implementation changes. The choice is between formal boundary maintenance (like glibc's ABI stability policy) or per-application Gutenberg isolation (like containers or WinSxS). There is no third option that gives you shared Gutenberg without boundary discipline.

---

## 8. gRPC: CORBA in a Hoodie

gRPC presents as modern — HTTP/2, Protocol Buffers, open source, Google-backed — while repeating the same Gutenberg/Semantic collapse as CORBA and SOAP.

**The IDL coupling.** Protocol Buffers `.proto` files are the semantic contract, but they generate language-specific stubs that couple the semantic layer to a specific generated Gutenberg implementation. Change the `.proto`, regenerate the stubs, redeploy everything that uses them. This is the WSDL problem with better tooling. The semantic contract lives in a generated artifact that must be kept in sync across every service that touches it. In a large microservices architecture this is the CORBA dependency graph problem, reproduced in Go and Kotlin.

**The opaque wire format.** Unlike REST/HTTP, which is text-based and human-readable at the Gutenberg layer, gRPC's wire format is opaque binary. You cannot inspect a gRPC call with curl or a browser. The Gutenberg layer requires semantic tooling to read — a Protocol Buffers decoder, grpcurl, a specific observability plugin. This is the XML/SOAP problem: a semantic format that requires semantic tooling to inspect, making the Gutenberg layer opaque to generic tools.

**The contrast with REST.** REST treats the network as a Gutenberg bytestream and puts the semantic layer in the HTTP verb, the URL, and the JSON body. No generated stubs. No IDL. A client can call a REST API with nothing but curl. The semantic contract is human-readable at every layer. gRPC requires both sides to share the same `.proto` version — tighter coupling, better performance, the same CORBA trap with better error messages.

**Where gRPC is justified.** Bidirectional streaming, high-throughput internal microservices, polyglot environments where the generated stubs actually help. These are real Use cases where the coupling cost is worth the performance gain. The mistake is using gRPC as a default for all service communication — the same mistake as using CORBA for everything in the 1990s.

**GraphQL** is the more Use-pull alternative: schema-defined like gRPC but text-based like REST, introspectable, with a query language that lets the Use side request exactly what it needs rather than accepting what the Def side decided to return. More Use-pull than either REST or gRPC, at the cost of server-side query complexity.

---

## 9. OpenSSL: When Breaking Changes Are Justified

OpenSSL is the cleanest example of a legitimate boundary reset. Security vulnerabilities in the Gutenberg layer — the cryptographic implementation — require API changes to fix. A stable boundary over a compromised implementation is worse than a breaking change that secures it.

**Heartbleed** (2014) was a buffer over-read in the TLS heartbeat implementation — a Gutenberg boundary bug (reading beyond the allocated buffer) with catastrophic semantic consequences (private key exposure). The fix required not just patching the vulnerability but rethinking the boundary design.

OpenSSL 1.1.x introduced a deliberate ABI break to make struct internals opaque. Applications that had been reaching directly into OpenSSL structs — bypassing the formal API, touching the Gutenberg layer directly — broke. The break was correct: those applications were violating the boundary, and the violation had enabled the vulnerability. The boundary was redrawn to make the violation impossible.

OpenSSL 3.x went further: deprecated legacy algorithms, introduced a provider model for cryptographic implementations (a formal Gutenberg/Semantic separation within OpenSSL itself), and changed how algorithm selection works. Each change is a boundary redraw justified by security and correctness, not by convenience.

**LibreSSL and BoringSSL** represent different bets on where the OpenSSL boundary should sit. Both pruned the API aggressively — breaking compatibility with code using deprecated or dangerous features — in exchange for a smaller, more auditable boundary surface. LibreSSL (OpenBSD) prioritises correctness and auditability. BoringSSL (Google) prioritises the specific subset of TLS used in Chrome and Android. Both are musl to OpenSSL's glibc: smaller boundary, more predictable, less free-upgrade benefit, more stability and clarity.

The principle: **security vulnerabilities in the Gutenberg layer are the one category of breaking change that cannot be avoided by boundary discipline alone.** An API can be perfectly designed and still implement an algorithm with a side-channel vulnerability. When the Gutenberg implementation is the problem, the boundary must move. The Use signal here is not user feedback — it is a CVE.

---

## 10. WebAssembly: The Virtual Gutenberg Layer

WebAssembly (WASM) is the logical endpoint of the boundary evolution: a virtual Gutenberg layer that makes the underlying hardware completely invisible to the semantic layer above.

WASM defines a portable binary format and a virtual instruction set. Code compiled to WASM runs identically on any WASM runtime — browser, server, edge, embedded device. The Gutenberg layer (the real CPU, the real memory model, the real instruction set) is fully abstracted. The WASM runtime is the boundary implementation, and it is the same on every platform.

This is a more complete abstraction than Java's JVM — WASM has no class model, no garbage collector, no standard library baked in. It is as close to a portable bytestream as a compiled language can get while still being safe and sandboxed. The semantic layer (your code) is decoupled from the Gutenberg layer (the hardware) by a formal, versioned, specification-driven boundary that is identical everywhere.

**WASM as Rosetta generalised.** Apple's Rosetta translates x86 instructions to ARM at runtime — a specific, one-directional, platform-specific translation. WASM is a universal intermediate representation: compile once to WASM, run anywhere a WASM runtime exists. The translation from WASM to native instructions happens at the runtime, not at Apple's discretion. The boundary is open, specified, and not owned by any single vendor.

**DuckDB-WASM** is the example closest to this series. The same SQL engine, the same query planner, the same Friendly SQL extensions — running in a browser tab via WASM with no server required. The semantic layer (SQL, the DuckDB API) is identical to the native binary. The Gutenberg layer (browser WASM runtime versus native CPU execution) is completely different. The boundary held perfectly across the transition.

**WASI** (WebAssembly System Interface) extends this to the OS boundary — a POSIX-like interface for WASM programs that need filesystem access, networking, and process management. WASM + WASI is the portable binary future that Java promised in 1995 but could not deliver cleanly because it bundled too much semantic machinery (the class model, the GC, the standard library) into the Gutenberg layer. WASM stays close to bytes. The semantic layer is your responsibility. The Gutenberg layer is the runtime's responsibility.

Solomon Hykes (Docker co-creator) said in 2019: "If WASM+WASI had existed in 2008, we would not have needed to create Docker." Docker solves the Gutenberg/Semantic boundary problem by bundling the entire OS environment with the application — a heavy but reliable solution. WASM+WASI solves the same problem by defining a formal, minimal Gutenberg interface that any OS can implement — a lighter solution that keeps the boundary in one explicit place.

---

## 11. The Boundary Stability Spectrum

| Strategy | Boundary type | Free upgrade | Stability | Example |
|---|---|---|---|---|
| Static linking (musl) | Frozen at build | None | Maximum | musl libc, BoringSSL |
| Container | Frozen at build | On rebuild | High | Docker, OCI |
| Dynamic linking (glibc) | Managed ABI | Automatic | Medium | glibc, most Linux binaries |
| Managed platform | Formally versioned | Automatic | High | iOS, Android Mainline |
| Generated stubs (gRPC) | IDL-coupled | On regen | Low | gRPC, CORBA, SOAP |
| Shared DLL (unmanaged) | Informal | Implicit | Broken | Windows DLL Hell |
| WASM | Virtual Gutenberg | Runtime | Maximum | DuckDB-WASM, WASI |

The free upgrade column and the stability column trade off against each other — except for managed platforms and WASM, which achieve both by making the boundary formal, versioned, and independently maintainable.

---

## 12. The Newer Machine Test Revisited

Come back to the test from the start. Move your code to a newer machine. Does it just run faster?

- **Static-linked musl binary**: yes, always. The boundary is frozen. The new machine is faster.
- **Dynamically-linked glibc binary**: yes, usually. The glibc ABI is stable. The new machine is faster and glibc improvements are included.
- **Container**: yes, as long as the base image is compatible. Rebuild to get glibc improvements.
- **iOS app**: yes, with rare exceptions at major OS versions. Apple maintains the ABI.
- **Android app (modern)**: yes, for apps targeting current APIs. The Mainline modules are transparent.
- **gRPC service**: yes for performance, but the semantic contract is still coupled to the `.proto` version. Moving to a new machine does not fix the IDL coupling.
- **Windows app (pre-WinSxS)**: maybe. Depends on which DLLs the new machine has and whether they match.
- **WASM binary**: yes, always, everywhere. The virtual Gutenberg layer is identical on every machine.

The test reveals the boundary quality. A clean boundary means the code is indifferent to the Gutenberg layer it runs on — faster is better, more memory is better, SSD is better, and none of it requires a code change. A broken or coupled boundary means the code has opinions about the Gutenberg layer, and those opinions become maintenance debt every time the Gutenberg layer improves.

The goal of every boundary strategy in this post is the same: make the code indifferent to the Gutenberg layer while letting the Gutenberg layer improve freely. Unix got partway there with POSIX. Android got most of the way there through a decade of progressive formalisation. iOS had it by design. WASM completes the journey — a virtual Gutenberg layer so clean that the code above it has no Gutenberg opinions at all.
