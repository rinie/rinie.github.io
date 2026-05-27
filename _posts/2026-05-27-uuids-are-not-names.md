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

## The Pattern Is Everywhere

The UUID leak is not a software quirk. It is the universal failure mode of any system that confuses its Gutenberg identifier with its Semantic name. The same structure appears at every layer of daily life — and the physical world solved it before software existed.

**Email: the address versus the display name**

Every email has two identities. The Gutenberg address — `noreply@mg2.substack.com` — is what the mail server uses to route the message. The Semantic name — "Substack" — is what Outlook or Apple Mail shows in the From field. The boundary between them is the contact record or sender configuration that maintains the mapping.

When the mapping is absent or wrong, the Gutenberg address leaks into the display. You see `noreply@mg2.substack.com` instead of "Substack" and have to decide whether to trust it. This is the UUID smell applied to email: the system outsourced its naming problem to the recipient. It is also indistinguishable from a phishing email — a malicious Gutenberg address wearing a borrowed Semantic name — which is why the leak is not just inconvenient but a security problem. The boundary exists for a reason.

**Phone numbers: the number versus the address book**

Your phone's dialler works on Gutenberg addresses — E.164 numbers like `+31 46 123 4567`. The network routes on these. They are stable, globally unique, content-free. They are also meaningless to a human receiving a call from one they do not recognise.

The Semantic layer is your contacts app. "Mum" is not a phone number. It is a name your address book maps to one. The boundary is the contact record. When a number calls that is not in your contacts, the Gutenberg identifier surfaces — and you have to decide, from a string of digits, whether to answer. The system is doing the UUID leak in real time.

**Books: the cover and the barcode**

A physical book carries both identifiers on its own body, and the designer understood exactly where each belongs.

The front cover is the Semantic layer: title, author, cover art — everything a human uses to find, choose, remember, and recommend the book. *De Ontdekking van de Hemel*. Harry Mulisch. The Semantic identity is expressive, rich, and designed for human pattern recognition.

The back cover carries the Gutenberg layer: the ISBN barcode — `978-90-289-3987-4` — machine-readable, used by logistics systems, point-of-sale terminals, library catalogues, and inventory management. Content-free, globally unique, stable across every edition and translation.

Neither identifier is on the wrong side of the book. The boundary between them is the publisher's registry, the library catalogue, the bookshop database — all maintaining the ISBN-to-title mapping so that a barcode scanner in a warehouse and a reader browsing a shelf can each use the identifier that serves them.

A book cited only by ISBN in an academic paper is technically correct and practically useless to any human reader. The Gutenberg identifier leaked across the boundary.

---

## The Many-to-One Problem the System Ignores

There is a deeper failure mode that all of these examples share, and that most systems do not handle: **the same Semantic name can map to multiple Gutenberg identifiers, and the system rarely tells you which one it means.**

"Mum" in your phone contacts maps to one number. But "Mum" in someone else's contacts maps to a completely different number. The Semantic name is not globally unique — it is meaningful only within the context of the person who assigned it. The Gutenberg identifier is globally unique. The Semantic name is locally meaningful.

This becomes a system failure when the mapping is treated as if it were one-to-one and global:

- Your contacts app has "Mum" → `+31 46 123 4567`. Your sister's contacts app has "Mum" → the same number. Your mother's colleague has "Mrs Kervel" → the same number. Three different Semantic names, one Gutenberg identifier. The system that stores only the number and assumes the name is derivable from it has lost information that cannot be recovered without asking every person who stored it.

- An email system that indexes only on the Gutenberg address (`rinie@xs4all.nl`) cannot tell you that this address is "Rinie" to you, "Dad" to your children, and "Mr Kooijman" to his bank. The Semantic layer is personal and contextual. The Gutenberg layer is shared and universal.

- A book's ISBN is stable across every copy of every printing. But "that book about the angels" is a valid Semantic reference that resolves correctly for anyone who has read *De Ontdekking van de Hemel* — and resolves to nothing for anyone who has not. The Semantic name is meaningful only within a context of shared knowledge.

The system failure is assuming that the mapping is a function: one Gutenberg identifier → one Semantic name, universally. It is not. It is a relation: one Gutenberg identifier ↔ many Semantic names, each valid within a different context. The address book that does not let you add context — this is "Mum" *and* "Mrs Kervel" depending on who is asking — has modelled the boundary incorrectly.

Git handles this correctly. A commit SHA is a Gutenberg identifier. `main`, `v1.5`, and `fix/login-bug` are Semantic names — multiple names can point to the same commit, and the same name can point to different commits at different times. The ref system is explicitly a many-to-many mapping between Gutenberg identifiers and Semantic names. The system owns the mapping and keeps it consistent. The human never has to think about the SHA.
---

*Part of a series on the Gutenberg/Semantic model — the idea that every information system operates on two parallel layers: a physical/positional layer (bytes, addresses, routes) and a logical/meaningful layer (names, identities, content). Clean systems isolate the boundary between them in exactly one place.*
