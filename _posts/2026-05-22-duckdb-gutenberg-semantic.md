---
layout: post
title: "DuckDB: The Gutenberg/Semantic Model Done Right"
date: 2026-05-22
tags: [duckdb, sql, data, gutenberg-semantic]
level: technical
description: "DuckDB separates storage from SQL cleanly. Parquet on S3, Excel, CSV all queryable with the same SQL. The dual-track strategy done right."
---

This is part of a series. The [Gutenberg/Semantic model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) describes physical versus logical layers. The [Def-Use split](https://rinie.github.io/2026/05/16/def-use-lean-pull/) describes how authors and users inhabit different semantic models. This post applies both to DuckDB — one of the cleanest modern examples of getting the separation right.

---

## 1. The Gutenberg Layer: Columnar, Vectorized, Physical

DuckDB's engine is aggressively Gutenberg-layer clean. Every core design decision is about arranging bytes to match the physical access pattern of analytical queries.

**Columnar storage** — data is laid out physically by column, not by row. A query that reads `revenue` from ten million rows touches only the `revenue` column's bytes on disk. Row-oriented storage (Postgres, MySQL, most OLTP databases) would read every column of every row and discard the ones not needed. Columnar is a pure Gutenberg decision: arrange the physical bytes to match the Use. The semantic layer — your SQL query — is unaware of the layout.

**Vectorized execution** — DuckDB processes data in batches of ~2048 values using SIMD CPU instructions. The CPU's physical vector registers process 256 or 512 bits simultaneously. One instruction, many values, pure Gutenberg throughput. The semantic layer — the query plan — is unaware of the vector width. The same SQL runs on a CPU with 128-bit SSE2 or 512-bit AVX-512; the Gutenberg layer adapts, the semantic layer does not change.

**Single file, zero dependencies** — the entire database is one `.duckdb` file and one binary. No server process, no network configuration, no init scripts. The Gutenberg artifact is as minimal as possible. This is the git philosophy applied to databases: content-addressed, self-contained, portable.

**Parquet and Arrow as native Gutenberg formats** — Parquet is columnar, binary, self-describing at the schema level but not at the byte level. Arrow is the in-memory equivalent: a byte layout that matches the columnar execution model exactly. DuckDB treats both as first-class Gutenberg substrates — not import targets, not export formats, but native storage that the engine reads directly without conversion.

---

## 2. The Semantic Layer: SQL Extended Toward Actual Use

DuckDB's SQL interface sits cleanly on top of the Gutenberg engine. You declare *what* you want. DuckDB decides the Gutenberg execution: which columns to read, how to vectorize the aggregation, which files to scan, whether to push predicates into the Parquet reader.

```sql
SELECT region, sum(revenue)
FROM 'data/sales/*.parquet'
GROUP BY region
ORDER BY region
```

The SQL author never touches the physical layer. The boundary is a function call.

But DuckDB goes further than standard SQL — it extends the semantic layer in Use-pull directions, responding to what analysts actually need rather than what the SQL standard specifies.

### Friendly SQL

DuckDB's "Friendly SQL" extensions are the clearest Use-pull signal in its design. Standard SQL has accumulated decades of tribal Def decisions that make it unnecessarily hostile to human authors. DuckDB fixes them:

**`GROUP BY ALL`** — standard SQL requires you to list every non-aggregate column in `GROUP BY`, duplicating the `SELECT` clause:

```sql
-- Standard SQL: repeat yourself
SELECT year, month, region, sum(revenue)
FROM sales
GROUP BY year, month, region;

-- DuckDB Friendly SQL: say it once
SELECT year, month, region, sum(revenue)
FROM sales
GROUP BY ALL;
```

The Use signal: analysts writing aggregation queries constantly forgot to add a column to `GROUP BY` after adding it to `SELECT`. The error message was unhelpful. `GROUP BY ALL` removes the friction entirely.

**Column aliases in `WHERE` and `GROUP BY`** — standard SQL forces you to repeat expressions because aliases defined in `SELECT` are not visible in `WHERE` or `GROUP BY`. DuckDB allows them:

```sql
-- DuckDB: alias usable immediately
SELECT revenue * 1.21 AS revenue_vat, region
FROM sales
WHERE revenue_vat > 1000
GROUP BY region;
```

**`SELECT * EXCLUDE`** — one of the most requested analyst features. Standard SQL has no way to select all columns except a few. You must either list every column you want or use application-layer workarounds:

```sql
-- Standard SQL: list everything except the columns you don't want
SELECT id, name, email, region, revenue, created_at  -- tedious
FROM customers;

-- DuckDB: say what you don't want
SELECT * EXCLUDE (internal_id, etl_timestamp, row_hash)
FROM customers;
```

This is particularly powerful for join results where you want to drop the duplicate key column:

```sql
SELECT * EXCLUDE (o.customer_id)  -- already have it from customers
FROM orders o
JOIN customers c ON o.customer_id = c.id;
```

Standard SQL forces you to enumerate every column you want to keep. DuckDB lets you declare the exception — which is almost always smaller. The Use signal is clear: analysts working with wide tables (50-100 columns from data warehouses) spend significant time just listing column names. `EXCLUDE` removes the friction.

**`COLUMNS()` with regex** — select columns by name pattern:

```sql
SELECT id, COLUMNS('revenue.*')
FROM sales;
```

**Automatic `ORDER BY` in aggregates** — `list()`, `string_agg()` and similar aggregates accept `ORDER BY` inline:

```sql
SELECT region, string_agg(product, ', ' ORDER BY revenue DESC)
FROM sales
GROUP BY region;
```

All of these are Use-pull extensions. The SQL standard is the tribal Def. DuckDB listens to what analysts actually write and removes friction point by point. The same pattern as JSON5 fixing JSON's trailing comma problem — not redesigning the language, just removing the specific things the Use signal identified as painful.

---

## 3. Storage as Gutenberg: CSV, Parquet, Excel, S3

DuckDB's most radical Gutenberg/Semantic separation is in how it treats storage. Traditional databases own their storage format — the data lives in the database's pages, managed by the database's buffer pool, readable only through the database's interface. The Gutenberg layer (storage) and the semantic layer (queries) are coupled by design.

DuckDB decouples them completely. The semantic layer (SQL) can address any Gutenberg storage substrate directly:

**Local files:**
```sql
SELECT * FROM 'sales.csv';
SELECT * FROM 'sales.parquet';
SELECT * FROM read_csv('messy_data.csv', auto_detect=true);
SELECT * FROM read_parquet('data/*.parquet');   -- glob patterns
```

**S3 and object storage:**
```sql
SELECT * FROM 's3://my-bucket/data/sales.parquet';
SELECT * FROM 's3://my-bucket/data/year=2026/**/*.parquet';  -- Hive partitioning
SELECT * FROM read_csv('s3://my-bucket/exports/*.csv');
```

**Excel:**
```sql
INSTALL excel;
LOAD excel;
SELECT * FROM read_xlsx('report.xlsx', sheet='Sales Q1');
```

**HTTP directly:**
```sql
SELECT * FROM 'https://example.com/data/public.parquet';
```

**Multiple sources in one query:**
```sql
SELECT c.name, sum(o.revenue)
FROM 's3://crm-bucket/customers.parquet' c
JOIN 'data/orders/*.csv' o ON c.id = o.customer_id
GROUP BY ALL;
```

This is the Gutenberg/Semantic separation made fully operational. The semantic layer (SQL, joins, aggregations, filters) is completely orthogonal to the Gutenberg layer (where the bytes live and in what format). CSV on a local disk, Parquet on S3, Excel on a network share, JSON from an HTTP endpoint — the SQL is identical. Only the path changes.

The traditional database model collapses these layers: data must be imported into the database's Gutenberg format before the semantic layer can touch it. The import step is friction — a Gutenberg operation that adds no semantic value. DuckDB removes it. Read the bytes where they are, in whatever format they are in, and apply the semantic layer directly.

**Hive partitioning** is the Gutenberg addressing scheme for large datasets — `year=2026/month=05/day=21/data.parquet` encodes partition keys in the directory structure. DuckDB reads the Gutenberg path and injects the partition values into the semantic query automatically:

```sql
SELECT year, month, sum(revenue)
FROM read_parquet('s3://bucket/sales/**/*.parquet', hive_partitioning=true)
WHERE year = 2026
GROUP BY ALL;
```

The `WHERE year = 2026` predicate pushes down into the Gutenberg layer — DuckDB only reads the `year=2026/` directories. The semantic filter becomes a Gutenberg access optimisation transparently.

---

## 4. The Embedding Model: Boundary as a Function Call

DuckDB's decision to be an embedded library rather than a server is the architectural embodiment of the Gutenberg/Semantic principle.

Server databases (Postgres, MySQL, Oracle) have a Gutenberg layer — the server process, the network socket, the wire protocol — that the application must communicate through. The semantic layer (your query) crosses a process boundary and a network boundary to reach the execution engine. Every query pays serialization cost, network latency, and protocol overhead — Gutenberg friction added at the boundary.

DuckDB embedded runs in your process. The semantic layer and the Gutenberg engine share the same address space. No serialization, no network, no process boundary. The boundary is a function call — O(1) crossing cost regardless of data size.

This is a deliberate Use-pull decision: the primary Use case is analytical queries in data pipelines, notebooks, and applications — contexts where the overhead of a server process is pure waste. DuckDB removes the waste by making the Gutenberg/Semantic boundary as thin as physically possible.

---

## 5. DuckDB-WASM: Rosetta for the Browser

DuckDB-WASM is the Rosetta moment. The same semantic layer — SQL, the query API, all the Friendly SQL extensions — runs on a completely different Gutenberg substrate: the browser's WebAssembly runtime.

Your Observable Framework dashboard runs the same SQL against the same Parquet files whether the engine is native binary on a server or WASM in a browser tab. The semantic contract is stable. The Gutenberg substrate is swappable. The translation layer (the WASM runtime) handles the physical difference transparently — exactly as Rosetta handled the x86-to-ARM transition for macOS.

This enables the architecture at the heart of the Observable Framework + DuckDB stack:

- ETL writes Parquet files to `dist/data/` (Gutenberg: bytes on disk or S3)
- `manifest.json` signals new data availability (Gutenberg: a file with a timestamp)
- DuckDB-WASM in the browser queries the Parquet directly (Semantic: SQL over HTTP byte ranges)
- The browser renders the results (Semantic: visualisation layer)

No server query layer. No API. No serialization of query results. The browser fetches Parquet byte ranges (pure Gutenberg) and executes SQL locally (pure Semantic). The boundary is clean at every layer.

---

## 6. The Extension System: VS Code for Databases

DuckDB's extension system is the VS Code plugin model applied to databases. The core Gutenberg engine is stable and minimal. Semantic capabilities live in separately loadable extensions:

- `spatial` — geospatial types and functions
- `fts` — full-text search
- `excel` — Excel file reading
- `oracle` (your `duckdb-oracle`) — Oracle database connectivity via ODPI-C
- `httpfs` — S3 and HTTP file system access
- `json` — JSON reading and querying
- `parquet` — built-in, but the model is the same

Each extension adds semantic capability without touching the Gutenberg core. One failing extension does not crash the database. The core version is stable. Extensions evolve on their own schedule. This is the out-of-process isolation principle — not literally out-of-process here, but architecturally separated so the semantic additions cannot destabilise the Gutenberg foundation.

---

## 7. The SQL Tribal Def DuckDB Inherits

DuckDB is not entirely free of tribal Def. SQL is a decades-old standard with significant ecosystem investment, and DuckDB inherits its oddities:

- Positional `ORDER BY` (`ORDER BY 1, 2`) — a tribal Def that survives because the standard blessed it, not because it serves Use. Named columns (`ORDER BY region, revenue`) are clearer and safer but require more typing. DuckDB supports both — Use-pull enough not to break existing queries.
- `NULL` handling semantics — `NULL != NULL`, three-valued logic, `IS NULL` versus `= NULL`. Tribal Def from relational theory that confuses analysts consistently. DuckDB inherits it because breaking SQL compatibility costs more than the confusion.
- `HAVING` versus `WHERE` — a distinction that exists for historical reasons, partially dissolved by DuckDB's column alias support but not eliminated.

These are the places where the certification tribe (the SQL standard body, the ecosystem of existing tools) prevents DuckDB from following the Use signal all the way. The tribal Def is too embedded to dislodge. DuckDB adds Use-pull extensions on top rather than replacing the foundation — the JSON5 strategy rather than the TOML strategy.

---

## 8. Summary

DuckDB is one of the cleanest modern examples of the Gutenberg/Semantic separation done right:

- **Gutenberg layer**: columnar storage, vectorized SIMD, single file, Parquet/Arrow/CSV/Excel/S3 as native substrates — bytes wherever they live, in whatever format
- **Semantic layer**: SQL extended toward actual analyst Use — `GROUP BY ALL`, `EXCLUDE`, column aliases, `COLUMNS()`, friendly aggregates
- **Boundary**: a function call in the same process, or WASM for the browser — as thin as possible
- **Extension system**: stable Gutenberg core, semantic extensions at the boundary, the VS Code model for databases
- **Development culture**: Use-pull, listens to analyst feedback, ships pragmatic extensions over spec purity

The one place DuckDB does not escape the pattern is the SQL standard itself — a tribal Def too embedded to replace, worked around rather than replaced. But everything DuckDB controls, it gets right.

For analysts, data engineers, and anyone building dashboards or pipelines: DuckDB is what happens when the Gutenberg/Semantic boundary is respected from the first line of architecture. The bytes live wherever they live. The SQL works everywhere. The boundary is a function call. Move fast without breaking things.

---

## 9. The Dual-Track Strategy: Preserving the Tribal Def While Offering Better

The tribal Def is not just laziness or cowardice. Preserving backward compatibility is a genuine service to users who have existing code, existing pipelines, existing muscle memory. Breaking changes have a real cost — migration tax, broken tooling, Stack Overflow answers that suddenly give wrong advice. The tribal Def is the Gutenberg foundation: stable, load-bearing, the thing everything else rests on.

The dual-track strategy is more sophisticated than either pure tribal defence or breaking change:

- **The tribal Def stays** — existing code keeps working, the certification ecosystem does not break, migration cost is zero for those who do not move
- **The Use-pull alternative is added** — documented, idiomatic, gradually adopted by the community through quality rather than mandate
- **Voluntary migration** — pulled by the better experience, not pushed by a breaking change

JavaScript ES6 is the canonical example of this done well. `var` still works — the tribal Def is preserved. `const` and `let` are the Use-pull alternative. Nobody forced the migration; Prettier, ESLint, and community convention did it through Use-pull pressure. Today `var` is a code smell but it still runs everywhere.

The same pattern across the JS modernisation:

| Tribal Def (still works) | Use-pull alternative |
|---|---|
| `var` | `const` / `let` |
| `function` | arrow functions |
| `prototype` chains | `class` syntax |
| string concatenation | template literals |
| `require()` | `import` / `export` |
| `arguments` | rest parameters `...args` |
| `for` loops | `map` / `filter` / `reduce` |
| callbacks | promises → `async`/`await` |

In each case the old Def is preserved, the new Use-pull alternative is added, and the community migrates on its own schedule. No breaking change. No migration tax. Voluntary adoption pulled by quality.

DuckDB does the same:

| Tribal Def (still works) | Use-pull alternative |
|---|---|
| `ORDER BY 1, 2` | `ORDER BY region, revenue` |
| `GROUP BY year, month, region` | `GROUP BY ALL` |
| List every column explicitly | `SELECT * EXCLUDE (...)` |
| Repeat expression in WHERE | Column alias in WHERE |
| Standard JOIN column listing | `EXCLUDE` on join result |

**The key distinction from pure tribal Def defence:**

The tribe says: *the old way is correct, learn it.*
The dual-track says: *the old way still works, here is something better.*

One is Def-push. The other is Use-pull with backward compatibility as the Gutenberg foundation. The old code is the Gutenberg layer — preserved, stable, load-bearing. The new idioms are the semantic layer — expressive, evolving, shaped by Use feedback.

**Where the dual-track goes wrong** is permanent paralysis — two ways to do everything, neither deprecated, documentation split between them, beginners not knowing which to learn. JavaScript has this problem with `class` versus prototypes, `require` versus `import`, callbacks versus promises versus `async`/`await`. Three generations of async patterns all still valid, all still in Stack Overflow answers of varying age. The Use signal is fragmented because the Def never clearly signalled which track was preferred.

The Use-pull correction is what TypeScript's `@deprecated` and ESLint rules do — they make the old Def visible as legacy without removing it. The Gutenberg layer (the runtime) still supports it. The semantic layer (the tooling) signals that you should not use it. Voluntary migration with a nudge rather than a mandate.

DuckDB's version of the nudge is that Friendly SQL extensions lead in the documentation. The SQL standard is available if you need it for compatibility. `GROUP BY ALL` is what you see first in the examples. The tribal Def recedes without being removed. The Use-pull alternative becomes idiomatic through exposure rather than enforcement.

This is the mature form of the weak link willing to learn: not just listening to Use feedback and updating the Def, but managing the transition so that neither old users nor new users pay an unnecessary cost. The Gutenberg layer absorbs the compatibility burden so the semantic layer can evolve freely.
