---
layout: post
title: "After 25 Years SQL Still Wins: Tweaking Queries After the Architect Has Left"
date: 2026-06-11
tags: [sql, duckdb, gutenberg-semantic, moore's-law, waterline, def-push]
level: technical
description: "Hadoop, NoSQL, ORMs, object databases — all promised to improve on or replace SQL. All lost. SQL survived because the separation between what you ask (semantic) and how it executes (Gutenberg) was correct from the start. And the DBA tweaking queries five years after the architect left is the proof."
---

In 2024, Hannes Mühleisen — co-creator of DuckDB — gave a keynote at GOTO Amsterdam titled [A Short Summary of the Last Decades of Data Management](https://www.youtube.com/watch?v=-wCzn9gKoUk). He walked through thirty years of database history: the rise and fall of object databases, the Hadoop decade, the NoSQL wave, the return of SQL. The conclusion was not a prediction. It was an observation that had already happened: relational databases and SQL came out on top, repeatedly, across every challenge.

This post reads that history through the Gutenberg/Semantic lens and adds the perspective of the person who is actually there when the architect's design meets reality — five years later, with ten times the data, and a query that used to run in two seconds and now runs in forty.

---

## The Separation That Survived Everything

SQL has one architectural property that every challenger underestimated: it separates *what you want* from *how to get it*.

A SQL query is a semantic declaration. `SELECT region, sum(revenue) FROM sales WHERE year = 2024 GROUP BY region` says what the result should contain. It says nothing about which index to use, which join algorithm to apply, whether to scan the table or seek into it, how to parallelise the aggregation. Those decisions belong to the query optimiser — the Gutenberg layer — which makes them based on the current state of the data, the available indexes, the hardware, and the version of the engine running today.

This is the Gutenberg/Semantic boundary applied to data management. The SQL is the Semantic layer: stable, declarative, expressing intent. The execution engine is the Gutenberg layer: changing with every release, improving with every hardware generation, free to make different decisions as the data and the platform evolve.

The separation is what allowed SQL to survive thirty years of Moore's Law. Every time the hardware improved — faster CPUs, more RAM, NVMe replacing spinning disk, vectorised SIMD execution — the SQL stayed the same and the engine harvested the improvement. The query you wrote in 1999 runs faster today not because you changed it, but because the Gutenberg layer underneath it got better.

---

## The Def-Push Graveyard

Every major challenge to SQL in the last thirty years was a Def-Push that misidentified the problem.

**Object databases** — the 1990s bet that the semantic model (objects, inheritance, methods) should be the storage model. One layer, no separation. The object database stored your objects directly without the "impedance mismatch" of mapping them to rows and columns. The mismatch was real. The solution was wrong. When Moore's Law moved — columnar storage, vectorised execution, NVMe — object databases could not follow because their Gutenberg layer was semantically coupled. The iceberg was the wrong shape and could not be reshaped. They sank.

**Hadoop and MapReduce** — the 2000s bet that distributed processing was the correct response to data growth. A genuine Gutenberg insight: disk I/O was the bottleneck, spreading data across many machines increased aggregate bandwidth. The mistake was tangling the Gutenberg layer (the distributed execution) with the Semantic layer (the computation). You wrote Java that *was* both the query logic and the execution plan simultaneously. When NVMe arrived and a single machine could outperform a 2010 Hadoop cluster in raw I/O bandwidth, the Hadoop code could not harvest the improvement. The execution model was baked into the application. Mühleisen's talk makes this point directly: a modern laptop running DuckDB on a Parquet file outperforms the Hadoop cluster that processed the same data in 2012. The cluster was a Gutenberg solution to a Gutenberg problem that Moore's Law solved a different way — cheaper, faster, and without the Java.

**NoSQL** — the 2010s bet that SQL's relational model was the impediment to scale. Reject the schema, reject the joins, reject the query language, gain horizontal scalability and flexible data models. The Gutenberg reasoning was sound: horizontal scaling and schema flexibility are real properties. The semantic conclusion was wrong: removing SQL did not make the data problems easier, it made them harder. Every major NoSQL system eventually added SQL back. Cassandra has CQL. MongoDB has an aggregation pipeline and now SQL. DynamoDB has PartiQL. The semantic layer that SQL provides — declarative queries, the ability to express intent without specifying execution — turned out to be what developers actually needed. The Use signal was heard, eventually.

**ORMs and LINQ** — the 2000s-2010s bet that hiding SQL behind an object layer would make developers more productive. It did, at the cost of the optimiser's freedom. An ORM generates SQL that made sense when the schema was small and the data was thin. The same ORM generates the same SQL when the schema has grown and the data is large. The optimiser in 2024 could make much better decisions than the ORM's 2019 translation — if it could see the actual semantic intent. It cannot. The ORM made the decisions above the waterline and handed the optimiser a fait accompli. The waterline was hidden. Five years later, the architect who chose the ORM has left, and the DBA is looking at a forty-second query that used to run in two seconds.

---

## The DBA Tweaking Queries After the Architect Has Left

This is the practical heart of the Gutenberg/Semantic model applied to databases.

The architect designed the system. They chose the schema, wrote the queries, deployed the application. The application worked. The architect moved on.

Five years later: the data grew from one million rows to fifty million. The query optimiser was upgraded with the database version. The execution plan the optimiser chose in 2019 no longer exists — the new optimiser makes different decisions based on different cardinality estimates, different index statistics, different cost models. The query still returns correct results. It now takes forty seconds instead of two.

The DBA's job is at the waterline. Not in the application — that has not changed and should not change. Not in the schema — that is stable and load-bearing. At the resolver layer: the SQL, the indexes, the statistics, the execution hints.

**If the SQL was kept separate from the application**, the fix is surgical: find the query, analyse the execution plan, add an index hint or refresh the statistics or rewrite the predicate to help the optimiser understand the data distribution. Half a day. Query back to two seconds. Application untouched.

**If the SQL was embedded in the application** — string concatenation in a Java method, LINQ expressions compiled into the application binary, ORM-generated queries baked into the data access layer — the fix is much harder. The DBA must read the application code, understand the surrounding logic, change the ORM configuration or the LINQ expression, retest the application behaviour, redeploy. The same fix, ten times slower and ten times riskier.

The resolver — SQL as a named, separate artifact — is the seam that makes maintenance possible. Stored procedures and views are underrated for exactly this reason: they are named resolver artifacts. The application calls `get_active_customers()`. What that resolves to in SQL can be tuned, rewritten, or completely replaced without touching the application. The resolver is the waterline. The waterline is where the work happens.

---

## DuckDB and Friendly SQL: Use-Pull at the Query Layer

DuckDB is the positive proof of both principles simultaneously.

The engine is pure Gutenberg innovation: in-process, columnar, vectorised SIMD execution, Parquet and CSV and Excel and S3 as native storage substrates. Everything below the SQL waterline was redesigned around modern hardware — NVMe latency, CPU cache lines, SIMD registers. The Gutenberg layer is radically different from a 1990s row-oriented database.

The SQL waterline is clean and standard. The same SQL that ran on PostgreSQL runs on DuckDB. The same semantic intent — `SELECT`, `JOIN`, `GROUP BY`, `WHERE` — is preserved. The application does not change. The engine underneath it is completely different. The waterline held.

But DuckDB goes further. It listened to the Use signal — what analysts actually complained about in SQL — and extended the semantic layer in Use-Pull directions. Not breaking changes. Not a new query language. Extensions that remove friction point by point:

**`GROUP BY ALL`** — analysts constantly forgot to add a column to `GROUP BY` after adding it to `SELECT`. The error was unhelpful. `GROUP BY ALL` removes the friction without changing the semantics.

**`SELECT * EXCLUDE`** — joining two tables and wanting all columns except the duplicate key is a universal Use case. Standard SQL forces you to list every column you want. `EXCLUDE` lets you declare the exception. The resolver does the rest.

**Column aliases in `WHERE`** — standard SQL forces you to repeat expressions because aliases defined in `SELECT` are not visible in `WHERE`. A tribal Def preserved for decades for spec-compliance reasons. DuckDB acknowledged the Use signal and fixed it.

**`ATTACH` and direct file queries** — `SELECT * FROM 's3://bucket/data/*.parquet'` is the resolver principle applied to storage. The semantic intent (query this data) is separated from the Gutenberg carrier (where it lives and in what format). The analyst does not import, transform, or manage. They query. The engine resolves the Gutenberg details.

Each Friendly SQL extension is the weak link willing to learn: the Use signal (this is friction, this causes errors, this wastes time) heard and acted on, without breaking the existing semantic layer. Old queries keep working. New queries are cleaner. The dual-track strategy applied to a query language.

---

## Why SQL Will Still Be Here in 25 More Years

The architectural reason is simple: SQL is at the right level of abstraction.

It sits above the Gutenberg layer (storage, execution, hardware) without embedding it. The optimiser is free to change. The hardware is free to improve. The cloud provider is free to swap NVMe for whatever comes next. The SQL stays the same.

It sits below the application layer without conflating them. The application expresses business intent in code. The database expresses retrieval intent in SQL. The boundary is the API call — a clean seam with a well-defined contract on each side.

Every challenger that tried to replace SQL either moved the boundary in the wrong direction (object databases, ORMs moving it upward, tangling SQL with application logic) or removed the boundary entirely (Hadoop, NoSQL). Every challenger eventually discovered that the boundary was load-bearing — that the freedom to improve the Gutenberg layer independently, and the freedom to tune the resolver independently, were properties worth more than the problems they were trying to solve.

SQL is not winning because it is perfect. It is winning because the separation it embodies is correct. The DBA tweaking queries after the architect has left — that person is the proof. They can do their job because the waterline is there, visible, and at the right layer.

The architect built the application above the waterline. The DBA maintains the resolver at the waterline. The engine improves below the waterline. None of them have to touch each other's work. That is not an accident. That is why SQL is still here.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). The talk that inspired it: [A Short Summary of the Last Decades of Data Management](https://www.youtube.com/watch?v=-wCzn9gKoUk) by Hannes Mühleisen at GOTO Amsterdam 2024. Related: [DuckDB: The Gutenberg/Semantic Model Done Right](https://rinie.github.io/2026/05/22/duckdb-gutenberg-semantic/) on DuckDB's architecture, and [Revisiting the Waterline: Small Fixes, Five Years Later](https://rinie.github.io/2026/06/10/revisiting-the-waterline/) on platform drift and the inspection gap.*
