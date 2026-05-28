---
layout: post
title: "YAML, JSON, and the Config File That Fights Back"
date: 2026-05-21
category: "Software Architecture"
tags: [configuration, yaml, json, formats, devops]
depth: technical
---

This is part of a series. The [Gutenberg/Semantic model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) describes physical versus logical layers. The [Def-Use split](https://rinie.github.io/2026/05/16/def-use-lean-pull/) describes how authors and users inhabit different semantic models. This post applies both to the config files you edit every day.

---

## 1. The Boundary Problem in Config Formats

The [first post in this series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) established that a clean Gutenberg/Semantic boundary should be O(1) and content-independent. To find a boundary you should be able to look at a small, fixed number of bytes and know where you are. UTF-8 achieves this — continuation bytes always start with `10xxxxxx`, so you can find the next character boundary by scanning at most 3 bytes forward regardless of what came before.

YAML fails this test immediately. Indentation carries semantic meaning. The boundary between "structural byte" (a space that changes the parse tree) and "content byte" (a space inside a string) depends on context accumulated from the start of the file. One misplaced space silently changes the meaning of an entire subtree. The boundary problem is O(n) and content-dependent — exactly the pattern that makes backslash escaping and UTF-16 problematic.

This is not a minor inconvenience. It is a fundamental design property that makes YAML files unreliable to edit by hand, difficult to diff meaningfully, and dangerous to generate programmatically. The Gutenberg layer (whitespace, indentation) is doing semantic work, and there is no local way to verify it is doing that work correctly.

---

## 2. YAML: Def-Push All the Way Down

YAML was designed to be human-readable. The Def was generous — the designers wanted config files to look clean and uncluttered. No braces, no quotes where avoidable, indentation to show structure. The aesthetic Def is pleasant. The Use is treacherous.

**The Norway problem** is the most famous example. In YAML 1.1, the following values are parsed as booleans: `yes`, `no`, `on`, `off`, `true`, `false`, `y`, `n`. Norway's ISO 3166-1 alpha-2 country code is `NO`. A YAML config file with `country: NO` silently sets country to `false`. The Def (the parser's type coercion rules) overrides the Use (what you wrote and meant) without warning.

Other YAML surprises the Def introduces without asking:

- `0777` parsed as octal 511, not the string "0777"
- `1_000_000` parsed as the integer 1000000
- `2026-05-21` parsed as a date object, not a string
- Four different multiline string syntaxes (`|`, `>`, `|-`, `>-`) with subtly different whitespace and newline handling
- Implicit document markers (`---`) that interact with multi-document streams in non-obvious ways

Each of these is the Def making a semantic decision about your data without your consent. The parser has a complete model of what your values *mean* and applies it regardless of what you *intended*. This is Def-push at the byte level — the author of the YAML spec decided that `no` means `false`, and every user of every YAML file inherits that decision whether they want it or not.

YAML 1.2 fixed the boolean coercion problem. But the significant whitespace remains, the implicit type coercion remains for dates and numbers, and the four multiline string modes remain. The Def was updated at the spec level but the Use — millions of existing YAML files and parsers — was already locked in.

---

## 3. JSON: Designed for Machines, Forced on Humans

JSON is a different failure mode. It is not dangerous like YAML — it does not silently coerce your data into unexpected types. It is simply hostile to the Use case of humans writing and maintaining config files.

JSON was designed as a data interchange format: machine-generated, machine-parsed, transmitted over a network. Douglas Crockford's original Use case was JavaScript objects serialised for Ajax requests. The Def was appropriate for that Use. Then JSON got adopted for config files everywhere:

- `.eslintrc`
- `package.json`
- `tsconfig.json`
- `launch.json`
- `.prettierrc`
- Hundreds of others

The Use case changed. The Def did not.

**No comments** — the single most complained-about JSON limitation for config file use. The Use case of "explain why this value is set this way" is simply prohibited by the Def. Config files without comments are config files that lose their rationale the moment the person who wrote them leaves the team. Every workaround — a parallel `README`, a `_comment` key, an external wiki — is a Use finding its way around a Def that refuses to move.

**No trailing commas** — the most friction-generating limitation for human editing. Every person who has ever edited a JSON array has left a trailing comma and received a parse error. The Use is consistent and universal. The Def refuses to accommodate it because JSON's grammar was specified without it.

**Forced double quotes on every key** — verbose, noisy, and unnecessary for identifiers that are unambiguous without them. `{"name": "value"}` versus `{name: "value"}` — the quotes add nothing for the human reader and exist only because the Def requires them.

**No multiline strings** — forces escape sequences (`\n`, `\"`) where a human would naturally use a literal newline or quote. Config values that contain prose, SQL, or template strings become unreadable.

JSON is a Gutenberg format — designed for the bytestream layer, machine-to-machine transmission, where strict unambiguous parsing matters more than human readability. It was pulled into the semantic layer (human-authored config files) without adapting to the new Use case. The tribe (the JavaScript ecosystem) adopted it because it was already there, it became the standard, and the standard became load-bearing.

---

## 4. JSON5 and TOML: Use-Pull Corrections

Both JSON5 and TOML are Use-pull responses — they listened to what people actually complained about and removed the friction point by point.

**JSON5** fixes JSON's human-editing problems while staying recognisably JSON:

- Trailing commas allowed in arrays and objects
- Single-quoted strings allowed
- Comments allowed (`//` line comments and `/* */` block comments)
- Multiline strings via backslash line continuation
- Unquoted keys where unambiguous identifiers
- Hexadecimal literals, leading/trailing decimal points

JSON5 is the weak-link-willing-to-learn response to years of JSON config file complaints. It does not redesign the format. It removes the specific friction points the Use signal identified, one by one. The result is backward-compatible enough that most JSON is valid JSON5, and the delta is exactly the set of things humans complained about.

**TOML** goes further — it rejects deep nesting as the primary structure and matches its Def to the actual Use case of config files:

Most config files are flat or shallowly nested key-value pairs with some grouping. Deep nesting is rare. TOML's structure matches this:

```toml
# This is a comment — allowed everywhere
title = "My Application"
version = "1.0.0"           # trailing comment

[database]
host = "localhost"
port = 5432
name = "mydb"

[server]
host = "0.0.0.0"
port = 8080
```

Versus the equivalent JSON:

```json
{
  "title": "My Application",
  "version": "1.0.0",
  "database": {
    "host": "localhost",
    "port": 5432,
    "name": "mydb"
  },
  "server": {
    "host": "0.0.0.0",
    "port": 8080
  }
}
```

TOML is also self-synchronizing in the Gutenberg sense — you can read any line and know what it means without the surrounding indentation context. `key = value` is always a key-value pair. `[section]` is always a section header. The boundary problem is O(1) local, not O(n) context-dependent. No significant whitespace. No silent type coercion surprises. Explicit types where needed (`2026-05-21` is a date only if you write it under a `[date]` key typed as date).

---

## 5. The Format Design Pattern Table

| Format | Designed for | Used for | Key failure for human Use |
|---|---|---|---|
| YAML | Human-readable config | Everything | Significant whitespace, silent coercion, Norway problem |
| JSON | Machine interchange | Human config | No comments, no trailing commas, forced quotes |
| JSON5 | Human-edited JSON | Config files | None — fixes the Use complaints |
| TOML | Human config | Config files | Awkward for deep hierarchies — but that's the right tradeoff |
| INI | Human config | Simple config | No standard, no nesting, inconsistent parsers |

The pattern: formats designed for machine Use get forced onto human Use without adapting. Formats designed for human Use from the start (TOML, INI) trade machine-parsing elegance for human-editing ease. JSON5 is the pragmatic middle — a Use-pull patch on an existing Def rather than a new Def designed from the right starting point.

---

## 6. The Certification Tribe Again

JSON became dominant not because it was the best config format but because it was blessed by the JavaScript ecosystem and built into every runtime. The tribe adopted it, it became the standard, and the standard became load-bearing. `package.json` is not going away. `tsconfig.json` is not going away. The tribal Def is too embedded to dislodge even when TOML or JSON5 is objectively better for the Use case.

This is the same pattern as MDI, Nokia, and car infotainment. The format that wins the early adoption race gets embedded into tooling, documentation, tutorials, and tribal identity. Later formats with better Use-side design cannot overcome the switching cost even when the Use signal is unambiguous.

The small victories are telling: Rust chose TOML for `Cargo.toml` — a new ecosystem with no prior tribal commitment, free to read the Use signal directly. Python chose TOML for `pyproject.toml` after years of `setup.cfg` and `setup.py` pain. New entrants with no tribal investment pick the better format. Established ecosystems defend the incumbent.

---

## 7. Significant Whitespace as a Def-Push Tell

YAML and Python share significant whitespace as a design choice, and both attract the same critique: the Gutenberg layer (indentation, newline position) carries semantic meaning, which means the boundary between structure and content is context-dependent rather than local.

This is not just an aesthetic complaint. It has practical consequences:

- **Diffs become unreliable** — moving a block of YAML changes its meaning without changing any of its content bytes. A diff that shows no content changes can represent a major semantic change.
- **Generation is fragile** — programmatically generating YAML requires tracking indentation state, which is Gutenberg work leaking into the generation layer
- **Editors fight the format** — every YAML editor needs special indent-aware handling that plain text editors handle poorly. The format requires semantic tooling to edit reliably.
- **Copy-paste is dangerous** — pasting a block of YAML from one context into another silently changes its meaning if the indentation level changes

The Gutenberg/Semantic boundary should protect you from these problems. YAML's significant whitespace collapses the boundary — making the Gutenberg layer (bytes, positions) semantically load-bearing in a way that cannot be locally verified.

Trailing commas and comments are the inverse: uses that humans naturally produce which the format's Def prohibits. The Use signal is in every JSON error message that says "unexpected token }" after a trailing comma. The Def refuses to learn.

JSON5 and TOML both say: the Use signal is clear, the Def should move. That is the weak link willing to learn, applied to file format design.

---

## 8. C Strings, Explicit \n, and the O(1) Boundary Principle

Config formats are a special case of a more general principle that applies to programming languages themselves: **the only legitimate O(n) boundary in a well-designed language is the block comment. Everything else should have O(1) local boundary detection.**

C's approach to multiline strings is the cleanest solution in mainstream language history:

```c
const char *sql =
    "SELECT id, name "
    "FROM users "
    "WHERE active = 1 "
    "ORDER BY name";
```

Each string token is self-contained — `"..."` starts and ends on the same line, O(1) boundary detection. The concatenation of adjacent string literals is handled at compile time by the preprocessor — pure Gutenberg, zero runtime cost. The string is broken across lines for human readability without changing the Gutenberg structure at all. A diff of this code is clean: changing one line changes exactly one substring, no indentation ripple effects.

**`\n` is a feature, not a limitation.** It makes the Gutenberg/Semantic relationship explicit at the point of authorship. When you write `\n` you are saying *I intend a newline here*. It is visible. It is local. It is unambiguous. When Python's triple-quoted string embeds a newline because you pressed Enter, the Gutenberg position (the physical line break) is carrying semantic weight silently — exactly the YAML significant whitespace problem, just inside a string literal. The exception is made invisible. The magic is implied.

Compare:

**Python triple-quoted — invisible magic:**
```python
sql = """SELECT id, name
FROM users
WHERE active = 1"""
```
The newlines are in the string content because of where the Gutenberg bytes happen to fall. Indentation is now dangerous — adding leading spaces to align the continuation adds spaces to the string. A diff may show whitespace changes that affect runtime behaviour.

**JavaScript template literals — nested unbounded parsing:**
```javascript
const sql = `SELECT id, name
FROM users
WHERE active = 1`;
```
Same newline significance problem, plus interpolation (`${}`) means boundary detection now requires parsing arbitrarily nested expressions. Not O(1). Not local.

**C adjacent tokens — explicit, local, safe:**
```c
const char *sql =
    "SELECT id, name\n"
    "FROM users\n"
    "WHERE active = 1";
```
Every boundary is O(1). Every newline is intentional and visible. Indentation of the source code is irrelevant to the string content. The Gutenberg layer (byte positions, line breaks) is completely decoupled from the semantic layer (string content).

### The O(1) boundary scorecard

| Construct | Boundary detection | Notes |
|---|---|---|
| C `"..."` per line | O(1) | `\"` escapes quote inside |
| C adjacent string tokens | O(1) per token | Concatenation at compile time |
| C `/* */` block comment | O(n) | The one legitimate exception |
| C `//` line comment | O(1) | Ends at newline, always local |
| Python `"""..."""` | O(n) | Must scan for closing `"""` |
| JS template literal | O(n) | Must parse nested `${}` |
| YAML `|` block | O(n) + context | Indentation-dependent end |
| Regex `/pattern/` | O(n) | Must find unescaped closing `/` |

The block comment (`/* */`) is the one honest exception — you genuinely need to scan forward to find the end, and both author and parser know it. Every other O(n) construct is a Def-push decision that pushes Gutenberg work (boundary detection) onto the semantic layer (the parser, the human reader, the diff tool).

---

## 9. Brace Matching versus Def-Clever Closing Keywords

The same O(1) principle applies to block delimiters. Braces (`{}`), brackets (`[]`), and parentheses `()` are the cleanest possible block delimiters:

- **O(1) to open**: see `{`, you are inside a block
- **O(1) to close**: see `}`, the block ends here
- **Symmetric and universal**: the same characters close every kind of block — function body, conditional, loop, object literal, array
- **Tool-friendly**: every editor since the 1970s can highlight matching braces, jump to matching brace, fold at braces. The Gutenberg structure is locally readable by both humans and tools.
- **Language-agnostic**: `{}` means block in C, C++, Java, JavaScript, Rust, Go, TypeScript, CSS, and dozens more. The semantic convention is stable across the entire ecosystem.

The alternatives — closing keywords — are Def-clever solutions that make the author feel expressive and the reader do more work:

**Shell/Bash:**
```bash
if [ condition ]; then
    do_something
fi                    # if — backwards. Clever. Unreadable.

case $var in
    pattern) action;;
esac                  # case — backwards. Same pattern, same problem.
```

**Ada/Pascal:**
```pascal
if condition then
    do_something
end if;               # verbose, but at least readable

case x of
    1: do_one;
end case;
```

**Ruby:**
```ruby
if condition
  do_something
end                   # at least it just says "end"

def method_name
  body
end                   # but which end closes which block?
```

**Visual Basic:**
```vb
If condition Then
    DoSomething()
End If                ' readable but verbose

For i = 1 To 10
    DoSomething()
Next i                ' Next i — annotated, but why?
```

The problem with `fi`, `esac`, `end if`, `end case`, `next i` is not just verbosity. It is that they require the reader to maintain **semantic context** to parse the **Gutenberg structure**. To know that `fi` closes a block you must remember you are inside an `if`. To know that `esac` closes a block you must remember you are inside a `case`. The Def is being clever — encoding the block type into the closing delimiter — but the Use pays with a higher cognitive load and tools that need language-specific parsers to do what brace matching does for free.

`}` is self-describing at the Gutenberg layer. It closes *something*. The editor can tell you what without understanding the language semantics. `fi` requires semantic knowledge to parse a Gutenberg boundary.

This is the same principle as `\n` versus embedded newlines. **Make the exception visible. Make the structure explicit. Do not rely on position or context for semantic meaning.** The Def that encodes structure into content — indentation, reversed keywords, position-dependent meaning — is pushing Gutenberg work into the semantic layer and calling it expressiveness.

C got the brace convention right in 1972. Bash got `fi` and `esac` wrong in 1989. The Use signal — decades of developers learning brace matching as a universal convention — has delivered its verdict. Languages designed since have almost universally chosen braces. The ones that didn't (Python, Ruby, shell) each paid a specific price in tooling complexity, diff noise, or cognitive load that brace-based languages avoided.

---

## 10. JavaScript: Tantalizingly Close

JavaScript got most of the boundary decisions right — `{}` for blocks, `//` and `/* */` for comments, `;` as the explicit statement terminator, trailing commas in arrays and objects (eventually). Then it undermined itself in exactly two places, both Def-clever decisions that create invisible magic.

**Automatic Semicolon Insertion (ASI)** is the more dangerous one. The Gutenberg boundary between statements should be explicit — `;` is O(1), local, unambiguous. ASI makes it context-dependent. The Gutenberg position (the line break) carries semantic meaning silently:

```javascript
return
  { value: 42 }
```

Returns `undefined`. Not the object. The parser inserts a semicolon after `return` because the next token is on a new line. The exact same YAML/Python significant whitespace problem, just at the statement level. The classic ASI trap:

```javascript
const a = 1
const b = 2
const c = a + b
(function() { console.log(c) })()
```

Parses as `b(function...)()` — `b` is called as a function because the `(` on the next line is interpreted as a function call continuation. The Gutenberg position was supposed to end the statement. ASI decided otherwise, silently.

`prettier` and ESLint's `semi` rule exist entirely to paper over this — enforcing explicit `;` everywhere so ASI never fires. The Use signal (this causes bugs) was clear. The tribe split into semicolon camp versus no-semicolon camp, a religious war that should never have existed. Both sides are working around a Def decision that should have been: **semicolons always explicit, always required, O(1) local boundary.**

**Parenthesis-free arrow functions** is the subtler one:

```javascript
const double = x => x * 2         // one parameter: parens optional
const add = (x, y) => x + y       // two parameters: parens required
const getObj = x => ({ value: x }) // returning object: parens required
```

Three different syntactic rules for the same construct depending on parameter count and return type. The Def saves two characters in the common case. The Use pays with inconsistency, style debates, and the object return case where `({...})` parens are required to stop `{` being parsed as a block body — another context-dependent Gutenberg boundary.

**What JavaScript got right** deserves acknowledgment: `{}` blocks, `//` and `/* */` comments, trailing commas (ES5+), `const`/`let` replacing `var`. TypeScript is essentially JavaScript admitting its Def-clever decisions created Use problems and adding a semantic layer to make the implicit explicit — the weak-link-willing-to-learn pattern at language design scale.

**The two fixes that would have made JavaScript ideal:**
- Remove ASI — require explicit `;`, error on missing. One rule, O(1), always.
- Require parens on all arrow functions — `(x) => x` always, no exceptions.

Both are breaking changes now. The tribal Def is too embedded. Prettier enforces both conventions in practice — a Use-pull tool imposing the correct boundary rules that the language itself refused to make mandatory.

---

## 11. Rust Implicit Return: Clever or Clear?

Rust inherits the expression-oriented design from functional languages — in Rust, almost everything is an expression with a value, and the last expression in a block is implicitly its return value:

```rust
fn double(x: i32) -> i32 {
    x * 2        // no return, no semicolon — implicitly returned
}

fn add(x: i32, y: i32) -> i32 {
    x + y        // same
}
```

With an explicit `return`:

```rust
fn double(x: i32) -> i32 {
    return x * 2;   // explicit, with semicolon
}
```

By the O(1) boundary and explicit-over-magic principles developed above, implicit return looks like a Def-clever decision — invisible magic, context-dependent meaning, the last expression carrying semantic weight by virtue of its Gutenberg position (being last) rather than an explicit marker.

**The case against implicit return** follows directly: the `;` is now semantic. `x * 2` returns. `x * 2;` does not — the semicolon turns the expression into a statement and discards the value. The Gutenberg character `;` is carrying semantic meaning (return versus discard) through its presence or absence. This is exactly the ASI problem in reverse — in JavaScript, a missing `;` is silently inserted; in Rust, a present `;` silently discards the return value.

A common Rust beginner mistake:

```rust
fn double(x: i32) -> i32 {
    x * 2;    // WRONG: semicolon discards value, function returns () not i32
}
```

The compiler catches this — Rust's type system makes the mistake a compile error rather than a silent bug. That is a meaningful difference from JavaScript's ASI, which fails silently at runtime.

**The case for implicit return** is that Rust's type system provides the oracle that JavaScript lacks. The return type is declared explicitly (`-> i32`). If the last expression's type doesn't match, the compiler errors immediately. The magic is not invisible — it is checked. The Gutenberg position (last expression) carries semantic weight, but the semantic layer (the type checker) validates it locally and loudly.

This is a genuine design tradeoff rather than a clear failure:

| | JavaScript ASI | Rust implicit return |
|---|---|---|
| Magic | Invisible, silent | Visible to type checker |
| Failure mode | Silent runtime bug | Compile error |
| O(1) boundary | No — context dependent | No — position dependent |
| Recoverable | Only with linter | Yes — compiler catches it |

**The honest verdict**: Rust implicit return is a Def-clever decision that is saved from being a Use problem by the strength of the type system. It violates the explicit-over-magic principle but gets away with it because the compiler makes the magic loud when it goes wrong. JavaScript ASI violates the same principle and gets away with nothing — the failure is silent and runtime.

The principle refines slightly: **make exceptions visible, or make violations compile errors. Silent runtime magic is the worst outcome.** Rust chose the second option. JavaScript chose neither.

Whether you prefer explicit `return` is ultimately a Use signal question — Rust's community has largely accepted implicit return as idiomatic, which is itself a Use verdict. The weak link willing to learn notes that the compiler is doing the editorial work that the syntax declined to do.
