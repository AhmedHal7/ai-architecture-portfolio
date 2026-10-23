# ADR-0004: Match messages to the board with references first, then hybrid search in PostgreSQL

| | |
| :--- | :--- |
| **System** | Baton (AI Delivery Sync & Feed for GitHub and Microsoft Teams) |
| **Status** | Accepted |
| **Date** | 2026-10-24 |
| **Deciders** | Ahmed Alhallag (AI architect) |
| **Principles** | [P1](../docs/architecture-principles.md#p1-ai-suggests-you-approve-code-applies), [P2](../docs/architecture-principles.md#p2-facts-come-from-the-source) |

## Context

The core AI job is to find the board item a message is about, or to decide that none exists. The board has about 650 items, and many have similar titles ("export timeout", "export performance"). A wrong match costs trust; a missed match creates duplicates (G5).

## Options considered

| Option | Good | Bad |
| :--- | :--- | :--- |
| 1. The model reads the whole board every time | Simple | About 100k tokens per message: slow, costly, hard to explain |
| 2. Vector search in a dedicated vector database | Good recall | Another system to run; weak on exact IDs and names |
| **3. References first, then hybrid search in our PostgreSQL (pgvector)** | Exact when there's a reference; explainable; one database | Embeddings must stay fresh; thresholds need tuning |

## Decision

**Option 3:**

1. **Explicit references win.** "#412", a PR link or a branch name links directly; no model is needed.
2. **Hybrid search** finds the top 10 candidates: keywords + vectors (pgvector) + filters on product, workstream and people.
3. **The model picks** one action from the fixed menu, with a confidence and a one-line reason.
4. **Confidence bands:** ≥ 0.8 shows a suggestion card; 0.5–0.8 shows a quiet "maybe related" list; below 0.5 offers "create new?" or ignores it.

## Consequences

* ✅ Cheap and explainable: every card shows its references and matched words.
* ✅ No new database: the board index lives in the PostgreSQL we already run.
* ⚠️ Embeddings must be refreshed when items change. *Mitigation:* re-embed on each board event, plus the nightly re-sync (ADR-0003).
* ⚠️ The thresholds need tuning. *Mitigation:* a labelled set of 100 messages, and "accepted without edits" tracked per suggestion type.

## Revisit when

The board grows beyond 50,000 items, or search takes longer than 500 ms at p95. Then move to a dedicated vector store.
