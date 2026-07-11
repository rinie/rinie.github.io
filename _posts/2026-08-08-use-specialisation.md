---
layout: post
title: "Use-Specialisation: The Gutenberg Pipe Stays Wide, the Use Side Narrows"
date: 2026-08-08
tags: [gutenberg-semantic, sql, api-design, pimpl, storage-compute, iceberg, ducklake, duckdb, resolver, evolvability, composability, http, redirects, parquet, hive-partitioning, use-pull]
level: technical
description: "The Def's job is to keep the pipe wide and never force the Use side to specialise before it's ready. But Use-specialisation is also taste — a resolver that watches which columns a consumer always selects, which fields it always destructures, can learn that pattern the way a barista learns your usual, without narrowing the pipe for anyone else. SELECT * EXCLUDE/RENAME, opaque handles, and Parquet's range requests are all the same wide pipe, narrowed only where the Use side actually reaches in."
---

[The junction box doesn't care what switch plate you eventually pick](https://rinie.github.io/2026/08/07/taste-comes-last/), because the electrician and the homeowner agreed on a box dimension generations before either of them showed up. The tempting way to translate that into software is "always name things explicitly, never accept anything wide or unspecified." That's the wrong lesson. The actual discipline is Use-specialisation: the row, the file, the object, the byte stream stays a Gutenberg pipe — wide, undifferentiated, carrying everything — for as long as possible, and it's the Use side, not the Def side, that reaches in and narrows it to specific fields, specific parameters, specific columns, at the exact point something is genuinely being consumed. The Def's actual job is to never force that narrowing early. A Def that specialises the pipe before the Use side asked for it isn't being careful. It's the thing this series keeps naming: forcing the Use side to adapt to the Def's schedule instead of its own is what makes software sux.

---

## SELECT * Is Fine — EXCLUDE and RENAME Are the Actual Mechanism

The instinct to replace `SELECT *` with a fully enumerated column list sounds like discipline. It's actually the opposite of the junction-box principle: a hardcoded list is the thing that needs maintenance every time the table legitimately grows and you want the new column to flow through a pass-through stage. The list is a wall you built, not a box you left open.

DuckDB's `EXCLUDE` and `RENAME` modifiers on `SELECT *` solve the two real failure modes of wildcard select without forcing full enumeration:

```sql
SELECT * EXCLUDE (internal_flag, created_by)
FROM orders
JOIN customers USING (customer_id)
```

This stays correct as either table gains columns. An unwanted new column doesn't silently leak through — it's excluded by name once, not by re-listing everything that *should* stay. A join's column-name collision gets fixed with `RENAME` instead of forcing the whole query into positional enumeration. The query stays wide by default and narrows only where narrowing is actually needed: excluding the specific thing that shouldn't pass through, renaming the specific thing that collides. That's Use-specialisation in one line of SQL — more robust to schema drift than a hardcoded list, not less.

A view is still a genuinely useful piece of indirection — a stable name consumers point at, letting the underlying table's storage evolve freely behind it — but the view's own definition can and often should use `SELECT * EXCLUDE (...)` rather than a fully enumerated list, for the same reason. The view's value is the stable name. It isn't a mandate to enumerate everything inside it.

---

## Parameters: Append, Don't Insert — and Prefer the Object

Positional parameters aren't the danger. Inserting a new parameter anywhere but the end is the danger, because every existing call site's arguments silently shift into the wrong slots. Append-only positional parameters are completely safe — old callers keep working, because nothing before their last argument ever moved.

The sharper tool in JavaScript, though, is reaching for an object or array argument instead of a long parameter list at all:

```javascript
// fragile the moment order changes
function createOrder(customerId, qty, discount, notes) { ... }

// stays wide, narrows only where used
function createOrder({ customerId, qty, discount = 0, notes }) { ... }
```

The object argument is Use-specialisation made structural — the object itself is a small Gutenberg pipe. A caller destructures only the fields it actually needs. A new field can be added to the object shape at any point without touching any existing call site, because nothing is positional and nothing is enumerated up front — the object stays whole, wide, and composable as it moves through the system, and specific fields only get pulled out at the point some function actually needs them. `{}` and `[]` aren't just convenient syntax. They're the same wide-until-consumed discipline as `SELECT * EXCLUDE`, just at the function-call boundary instead of the query boundary.

---

## Rows Evolve Like Pages — Don't Re-Narrow at Every Hop

A row passing through a pipeline should stay a row — a Gutenberg pipe carrying everything, undifferentiated, until something actually needs a piece of it. If a value arrives from a database query, moves through two or three transformation steps, and finally gets serialised to JSON or rendered to a screen, only that final step needs to know which specific fields matter. Every intermediate hop that insists on destructuring down to named fields and reconstructing a narrower shape is doing unnecessary work and creating unnecessary breakage — a new field added upstream now has to be threaded through every intermediate stage's explicit shape instead of just riding along inside the pipe until something actually needs it.

This is the same distinction [the fence post drew](https://rinie.github.io/2026/07/27/dont-hide-the-fence/) between a paragraph and a page: the row is a navigational unit, whole and composable, and only the point of actual display or consumption needs to turn into a specific, narrow shape — the way only the final rendered page needs to resolve to specific formatted content, while everything upstream of that can keep passing the whole unit along unexamined.

---

## The URL Layer Does This Too

A site's base URL is the same move applied to routing. [Every internal link on this site resolves against one configured base](https://rinie.github.io/2026/07/10/should-i-click/), not a value copy-pasted into every post — the whole site can move to a different domain and every link updates on the next deploy, because no individual post ever specialised down to a hardcoded destination. The base stays wide and abstract; the actual domain only gets resolved at build or request time, which is the point of use.

HTTP's 3xx redirects make the same promise explicit at the protocol level. A `301 Moved Permanently` or `308 Permanent Redirect` is a resolver saying: the old URL still works, here is where the resource actually lives now. A bookmark from 2019 keeps resolving correctly in 2026 because the old path was never narrowed into a dead end — it stays a valid, wide entry point that gets specialised to a current location at the moment someone actually requests it. This is Use-Pull, not Def-Push: the Def side moved the resource, but it left a resolver behind rather than forcing every existing link, bookmark, and search-engine index to update in lockstep. The Use side pulls the current location whenever it happens to ask, on its own schedule, not the Def's.



## Opaque Handles Are the Same Move, Not a Different One

C++'s ABI problem looks like the exception to all of this — a struct's binary layout really is fixed at compile time, with no runtime flexibility to fall back on. But PIMPL and opaque handles are still Use-specialisation, just enforced by the compiler instead of by convention. Oracle's ODPI-C never exposes `dpiConn`'s internal layout; it hands out an opaque pointer and a set of accessor functions. The consumer stays maximally wide — it holds a handle that could point at any internal layout — and only narrows down to a specific field by calling a specific accessor, at the exact point it needs that value. The struct behind the handle is free to grow because nobody outside the library ever specialised against its layout early. Same principle. Different enforcement mechanism.

---

## Storage and Compute: Wide Files, Narrow Queries

The pattern repeats at the architectural scale. A Parquet file, or an open table format sitting on top of one, stays wide — every column, every row, available — and it's the query issued against it that narrows down to what's actually needed, at the point of actual use. Which engine issues that query is a separate, late decision: DuckDB, Spark, Snowflake, Trino can all point at the same files, because the storage layer was never narrowed to fit any one of them.

Iceberg, Delta Lake, and DuckLake formalise this. Iceberg and Delta track schema and snapshot history in scattered JSON and Avro manifest files any compliant engine can read. DuckLake keeps that same catalog information in a proper SQL database instead, giving the "what schema is current" question its own stable, queryable interface rather than something a reader reconstructs by crawling files. All three exist so [no single compute vendor can lock you in by controlling both sides of what should stay an open seam](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/), and all three support additive schema evolution — a new nullable column doesn't rewrite existing files or break a reader still narrowed to the old shape.

Parquet itself does this at two granularities simultaneously, and it's worth spelling out because it's the cleanest example in this whole post. Hive-style partitioning — a directory layout like `year=2026/month=07/region=nl/data.parquet` — lets a query engine skip entire files without opening them, purely by matching the query's filter against the partition path. That's narrowing before a single byte is read: whole files stay unconsidered, wide and untouched, because the query only asked about `region=nl`. Inside a single file, Parquet's columnar layout plus its footer metadata lets an engine issue HTTP range requests for just the byte ranges holding the specific columns and row groups a query actually needs — not the whole file, a slice of it, resolved at query time. A dataset can sit as static files on plain object storage or a CDN, fully wide, every column and every partition present, and a query engine anywhere narrows it down to exactly the bytes it needs, on demand, per request. Nothing about the storage layer had to anticipate which columns any future query would want. That's Use-specialisation at the level of individual bytes on the wire — the file is the Gutenberg pipe, and the query is the Use side reaching into it.

Oracle sits outside this pattern by design, not by failure — storage and compute share a process, so there's no independently late-specialisable faucet at all. That's a legitimate, different architectural choice: tight coupling instead of a wide interface narrowed at the point of use.

DuckDB querying a raw CSV or Excel file directly extends the same principle one step further. `SELECT region, SUM(amount) FROM 'sales.xlsx' GROUP BY region` runs with no schema declared in advance, no import step, no staging table — the file stays exactly as wide and unstructured as it already was, and the SQL query is where the actual narrowing happens, at the point of use, same as querying a table.

---

## The Narrowing Itself Is Learned

There's one more thing worth naming before closing this out: Use-specialisation isn't just late, it's taste — in the exact sense [the barista knows your usual](https://rinie.github.io/2026/07/31/smart-coffee-machine-usual-cuppa/). The point of use isn't a single generic event that happens the same way for everyone. Which columns a particular consumer always selects, which fields a particular caller always destructures, which byte ranges a particular query engine always requests for a particular dashboard — all of that is a pattern, repeated by one Use side over time, and a resolver that's actually watching can learn it.

This is the same move as ART's device-specific AOT compilation, resolved fresh for the hardware asking, or a barista who starts making your order before you've said a word. A wide Gutenberg pipe stays available to everyone in full, and a resolver sitting at the boundary can notice that this particular consumer always narrows to the same six columns, always destructures the same three fields, always filters the same partition — and start optimising for that specific pattern without narrowing the pipe itself for anyone else. The taste is real, it's specific to one Use side, and it's discovered the same way any taste in this series gets discovered: by watching what actually happens, repeatedly, rather than asking once and assuming the answer never changes.

---



## Use-Pull, Not Def-Push

`SELECT * EXCLUDE / RENAME` over a hardcoded column list. Append-only positional parameters, or better, an object argument — its own small Gutenberg pipe — destructured only where needed. A row passed whole through a pipeline instead of re-narrowed at every hop. A base URL and an HTTP redirect that let an old link keep resolving instead of dying the moment something moves. An opaque handle narrowed only via an accessor call. A wide Parquet dataset narrowed to exact bytes by partition pruning and range requests, at query time, per request. Every one of these is the same Gutenberg pipe, kept wide by a Def that refuses to force the Use side's hand.

The name for that move, in the series' own vocabulary, is Use-Pull rather than Def-Push. Def-Push forces the narrowing early and forces everyone downstream to move in lockstep — the enumerated column list that breaks the moment the table changes, the positional parameter inserted mid-list that silently shifts every caller, the hardcoded URL that 404s the instant something moves. Every one of those is a Def that specialised the pipe on its own schedule, before the Use side asked, and made the Use side pay for it. Use-specialisation is the opposite discipline: the Def keeps the pipe wide, builds a resolver at the boundary, and lets the Use side reach in and specialise exactly what it needs, exactly when it needs it, on its own terms. A Def that never forces that choice is a Def that doesn't sux — not because it tried harder, but because it left the narrowing where it always belonged.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Taste Comes Last](https://rinie.github.io/2026/08/07/taste-comes-last/) on the physical version of this principle, [Don't Erase the White Line. Don't Pour Concrete Either.](https://rinie.github.io/2026/07/27/dont-hide-the-fence/) on the row-as-page distinction, and [Borg's Arrogance](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) on which interfaces harden and which atrophy.*
