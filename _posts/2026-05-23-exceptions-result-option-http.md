---
layout: post
title: "Exceptions, Result/Option, and HTTP: Error Handling as Def-Use"
date: 2026-05-23
category: "Software Architecture"
tags: [error-handling, programming-languages, http, type-systems, rust]
depth: technical
---

This is part of a series. The [Gutenberg/Semantic model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) describes physical versus logical layers. The [Def-Use split](https://rinie.github.io/2026/05/16/def-use-lean-pull/) describes how authors and users inhabit different semantic models. This post applies both to error handling — where the Def-Use tension is most visceral. You feel it every time you write a try/catch that you don't know what to do with.

---

## 1. Exceptions: Def-Push Error Handling

Exceptions are Def-push error handling. The author of the called code decides:

- That an error occurred
- What type it is (the class hierarchy)
- That it should interrupt control flow
- How far up the stack it travels

The caller has no choice but to catch or propagate. The Def forces the Use to participate in a semantic model it may not share.

**Inheritance multiplies the problem.** When you catch `IOException` you silently swallow `FileNotFoundException`, `SocketTimeoutException`, and `PermissionDeniedException` — all the bugs the subclass authors put in the hierarchy. You inherit the entire taxonomy of failure whether you understand it or not. The Def of the exception hierarchy becomes load-bearing for your error handling whether you designed it that way or not. Every bug in every subclass is now your bug too.

**Checked exceptions** (Java) are the Def-push extreme — the compiler forces you to catch or declare. The Use signal over 25 years has been unambiguous: developers write `catch (Exception e) {}` to make the compiler stop complaining. The Def forced a behaviour that produced the worst possible outcome — silently swallowed errors everywhere. The tribe defended checked exceptions for years. C# removed them after watching Java. Kotlin removed them. The Use signal delivered its verdict; Java did not listen.

The deeper problem is that exceptions hijack control flow. A thrown exception is a goto that the caller did not write and cannot see at the call site. The Gutenberg structure of the code — the sequence of statements, the visible control flow — no longer matches the Semantic reality of what executes. The call site looks like a normal function call. It might be a long-distance jump to a catch block three frames up.

---

## 2. C's Return Codes: Gutenberg-Layer Signalling

C's `0`/non-zero convention is Gutenberg-layer error signalling — minimal, local, no control flow hijacking. The caller decides what to do. `0` means success. Any other value means something went wrong. The specific value carries additional information if you want it.

The strength: no hierarchy to inherit, no control flow surprise, the caller is completely in control of the handling strategy.

The weakness: nothing forces the caller to check. You can ignore the return value of `write()` and the compiler says nothing. The error is invisible until production — a Gutenberg signal that the semantic layer can simply not look at. `errno` compounds this: global mutable state, set by the last failing call, valid only until the next system call. The Gutenberg mechanism is too thin to enforce the semantic contract.

C's approach is the right instinct — errors as values, caller decides — but without the tooling to make ignoring them visible.

---

## 3. HTTP Status Codes: The Most Successful Error Design in Computing

HTTP status codes are used by billions of clients, understood universally, and have been stable for thirty years. That longevity is itself a Use signal: the design was right from the start.

**Why they work:**

**Categorised but not over-specified.** 2xx means success. 4xx means client error. 5xx means server error. 3xx means redirection. The category is the semantic signal — sufficient for most callers to act correctly. The specific code is additional information, not required to handle the response.

**The caller decides what to do.** A 404 might be an error or expected behaviour depending on the caller's context. A 404 when checking if a resource exists before creating it is success. A 404 when fetching a required dependency is failure. The server signals the fact. The client interprets the meaning. The Def provides the signal; the Use decides what it means.

**No control flow hijacking.** The response arrives. You check the status. You decide. No exceptions, no forced catch blocks, no inheritance hierarchy of HTTP error subclasses. The Gutenberg structure of your code — the sequence of checks — matches the Semantic reality of what executes.

**3xx redirection** is particularly elegant. It is not an error at all — it is a Gutenberg address change with a semantic hint about why. The resource has moved permanently (301), temporarily (302), or should be retrieved with a different method (303). The DNS resolver of HTTP responses: the semantic name (the URL you requested) resolves to a different Gutenberg address (the redirect target) transparently.

**Layered correctly.** Reverse proxies, CDNs, and load balancers understand the categories without understanding application semantics. A 5xx triggers a retry at the infrastructure layer without the infrastructure knowing anything about your domain. A 4xx is not retried — the infrastructure knows the client needs to fix something. The Gutenberg layer (infrastructure) acts on the category. The semantic layer (your application) acts on the specific code. Clean separation at every layer.

---

## 4. Rust's Result and Option: The Synthesis

Rust's `Result<T, E>` and `Option<T>` synthesise the best of C (explicit, caller decides) and HTTP (typed, categorised, forces acknowledgment) without the worst of exceptions (control flow hijacking, inheritance bugs).

```rust
fn read_file(path: &str) -> Result<String, IoError> {
    // caller must handle both Ok and Err
}

// Explicit handling — caller decides
match read_file("data.txt") {
    Ok(contents) => process(contents),
    Err(e) => handle_error(e),
}

// Explicit propagation — visible at every call site
fn process_file(path: &str) -> Result<(), IoError> {
    let contents = read_file(path)?;  // ? makes propagation visible
    let parsed = parse(contents)?;
    write_output(parsed)?;
    Ok(())
}
```

**No control flow hijacking** — errors are values, not interrupts. The Gutenberg structure of the code matches the Semantic reality of what executes.

**Forced acknowledgment** — the compiler warns on unused `Result`. You cannot ignore an error without explicitly saying so (`let _ = result`). The acknowledgment is visible in the code.

**No inheritance** — `Result<T, E>` is generic. There is no bug taxonomy to inherit. The error type is exactly what the function declares, nothing more.

**Caller decides** — `unwrap()`, `match`, `?`, `map_err()`, `unwrap_or_default()`, `ok()`. The Use chooses the handling strategy appropriate for its context.

**The `?` operator** is the sharpest design decision. It makes error propagation explicit at every call site — a visible Gutenberg marker showing where control might diverge — without the verbosity of a full `match` everywhere. It is the `\n` of error handling: explicit where it matters, concise where it doesn't. A function that uses `?` throughout is auditable — you can see every point where an error might propagate without reading the called functions.

**`Option<T>`** applies the same model to nullability — Tony Hoare's "billion-dollar mistake." `null` is the ultimate invisible Def-push: the author of the function decided to return nothing, and the caller discovers this at runtime with a NullPointerException three stack frames away from the actual cause. `Option<T>` forces the caller to acknowledge the absence case at the call site, not three frames later. The Def makes the exception visible. The Use handles it locally.

---

## 5. Go's Multiple Return: Explicit but Not Enforced

Go takes a different path — multiple return values as the error signalling mechanism:

```go
contents, err := readFile("data.txt")
if err != nil {
    return err
}
```

The strength: explicit, visible at every call site, no control flow hijacking, no inheritance. The Gutenberg structure matches the Semantic reality.

The weakness: nothing enforces checking. You can write `contents, _ := readFile("data.txt")` and discard the error with `_`. The compiler does not object. This is C's `errno` problem in modern clothing — the mechanism is right, the enforcement is absent.

Go's Use signal has produced a cultural convention — the `if err != nil` pattern is so ubiquitous it has become idiomatic — but convention is weaker than compiler enforcement. Rust's `Result` with compiler warnings is the stronger design. Go's multiple return is the Use-pull compromise: explicit enough that errors are visible, not so strict that the language becomes cumbersome.

---

## 6. The Full Comparison

| Approach | Caller control | Forced acknowledgment | Inheritance bugs | Control flow |
|---|---|---|---|---|
| C return codes | Full | None | None | None |
| C errno | Full | None | None | None |
| Java checked exceptions | None | Compiler forced | Yes — catches parent catches all | Hijacked |
| Java unchecked exceptions | None | None | Yes | Hijacked |
| HTTP status codes | Full | By convention | None | None |
| Go multiple return | Full | By convention | None | None |
| Rust Result / Option | Full | Compiler warning | None | Explicit with `?` |

The progression is a Use-pull learning curve across the industry:

- **C's Use signal**: ignored return codes cause bugs → **Def response**: exceptions force acknowledgment
- **Exceptions' Use signal**: catch blocks swallow errors, inheritance creates surprises → **Def response**: checked exceptions force declaration
- **Checked exceptions' Use signal**: `catch (Exception e) {}` everywhere, worse than before → **no response from Java**; response came from Rust, Kotlin, Swift, Go, all independently converging on explicit error values
- **HTTP's signal**: 30 years of stable adoption → no pressure to change because the design was right from the start

The convergence on `Result`/`Option` across Rust, Swift, Kotlin, Haskell, Elm, and others is the Use signal delivering its verdict across an entire ecosystem. The Def (exceptions plus inheritance) was wrong. The community found the better answer and migrated toward it voluntarily — the dual-track strategy at language ecosystem scale.

---

## 7. Inheritance Itself as the Pattern

The error handling story is a specific instance of a more general problem with inheritance: **you inherit the entire Def, including all the parts you did not choose.**

A class that extends `BaseService` inherits not just the interface contract but every implementation decision, every bug, every side effect, every dependency the base class introduced. The inheritance hierarchy is the tribal Def made load-bearing. When `BaseService` changes, every subclass is affected whether the change is relevant to them or not.

Rust's traits, Go's interfaces, and TypeScript's structural typing all solve this the same way: **describe what you need, not what you are.** A trait or interface says "I need something that can do X" — a semantic contract, locally declared, without inheriting a Gutenberg lineage of implementation decisions.

```rust
trait Readable {
    fn read(&self) -> Result<String, IoError>;
}

fn process(source: &impl Readable) -> Result<(), IoError> {
    let data = source.read()?;
    // ...
}
```

`process` declares what it needs. It does not inherit what `Readable` implementations happen to carry. The Use defines the interface. The Def implements it. The direction is right.

This is the editor principle from the [Def-Use post](https://rinie.github.io/2026/05/16/def-use-lean-pull/) applied to type systems: the caller is the editor, declaring the semantic contract it needs. The implementation is the journalist, providing the content. Inheritance collapses the two — the journalist writes the headline, the article, and the editorial policy simultaneously, and every reader inherits all three whether they wanted them or not.

---

## 8. The Lesson

Error handling is a microcosm of the entire Def-Use framework:

- **Exceptions** are Def-push: the author decides what is an error, how it propagates, and forces the caller to participate in their taxonomy
- **C return codes** are Gutenberg-minimal: right instinct, too thin to enforce
- **HTTP status codes** are Use-pull by design: categorised, caller decides meaning, no control flow surprise, layered correctly at infrastructure boundaries
- **Rust Result/Option** is the synthesis: explicit, typed, caller decides, compiler-enforced, visible at every propagation point

The Use signal across thirty years of exception-based programming has been consistent: developers write empty catch blocks, inherit bugs they didn't know about, and discover null pointer exceptions at runtime three frames from the cause. The Def (exceptions, checked exceptions, inheritance hierarchies) kept defending itself. The Use found better answers in Result/Option and the ecosystem followed.

The `?` operator is the closing argument: error propagation made explicit, local, auditable, and concise simultaneously. The exception's invisible goto replaced by a visible Gutenberg marker. Make the exception visible. Make the structure explicit. Do not rely on position or context for semantic meaning.

The config file lesson, the brace matching lesson, and the error handling lesson are all the same lesson.
