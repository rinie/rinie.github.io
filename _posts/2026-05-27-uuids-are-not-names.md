---
layout: post
title: "UUIDs Are Not Names"
date: 2026-05-27
---

A UUID is a perfect Gutenberg artifact. It is globally unique, content-free, and meaningful only to the system that issued it. It belongs in a database primary key, a foreign key reference, a cache entry, a log line. It does not belong in a conversation with a human.

When a system hands a UUID to a user and says "you will need this to continue," it has pushed a Gutenberg artifact across the boundary into the Semantic layer. The user is now expected to maintain a mapping that the system should own.

---

## The Leak

It happens everywhere:

- AWS gives you `i-0a1b2c3d4e5f67890` for your server. You name it `prod-api-1` in a spreadsheet.
- Jira gives you `PROJ-8472`. Your team calls it "the invoicing bug."
- Stripe gives you `cus_NffrFeUfgR3uDm`. Your support team writes "NffrFeUfgR3uDm = Acme Corp" in a Notion doc.
- A chat system stores conversations as `176df256-de57-4a05-b9fd-1bcef35c194d`. The user calls it "Pages versus chapters."

In every case the user is maintaining a local DNS — a private mapping from Gutenberg artifact to Semantic name — because the system outsourced its naming problem.

This is a Def-Push: the system defines identifiers on its own terms and hands them to the Use side to deal with. The Use side always builds a workaround, because humans cannot think in UUIDs.

---

## DNS Solved This in 1983

The Domain Name System is the canonical solution to exactly this problem. A hostname (`raspi3.home`, `api.example.com`) is a stable Semantic name. An IP address (`192.168.1.53`, `203.0.113.42`) is a Gutenberg location. DNS maintains the mapping. The human uses the name. The network uses the address. The boundary is in one place, owned by one system.

The insight generalises completely:

| Gutenberg | Semantic | Boundary |
|---|---|---|
| IP address | hostname | DNS |
| Inode number | filename | filesystem |
| Git SHA | branch/tag name | git ref |
| UUID | human-readable title | application layer |
| `PROJ-8472` | "the invoicing bug" | Jira itself, if it tried |

Every row is the same pattern. The Gutenberg artifact is the stable, content-free key. The Semantic name is the human-readable handle. The boundary system maintains the mapping so neither side has to.

Git does this well. You never need to remember `a3f8c21`. You use `main`, `v1.5`, `fix/login-bug`. The SHA exists and is important — it is the content-addressed Gutenberg identity — but it stays below the boundary. You only see it when you explicitly ask.

---

## The Smell

The diagnostic is simple: if a user has to copy a system-generated identifier into a document, a spreadsheet, or their own memory in order to find something later, the system has leaked a UUID across the boundary.

The fix is always the same: let users assign Semantic names, maintain the UUID-to-name mapping internally, and expose only names in the interface. The UUID can remain as the stable Gutenberg key — immutable, collision-free, safe to use in foreign keys and log lines — while the name floats above it as the human handle.

A conversation titled "Pages versus chapters" is findable by a human. A conversation identified as `176df256-de57-4a05-b9fd-1bcef35c194d` is findable only by a machine that already knows it. Storing both, exposing only the former, is not a difficult engineering problem. It is a boundary problem — and the boundary has a known solution.

UUIDs are excellent Gutenberg identifiers. They are terrible names. The system that issued them should be the last place they are visible.

---

*Part of a series on the Gutenberg/Semantic model — the idea that every information system operates on two parallel layers: a physical/positional layer (bytes, addresses, routes) and a logical/meaningful layer (names, identities, content). Clean systems isolate the boundary between them in exactly one place.*
