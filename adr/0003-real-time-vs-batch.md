# ADR-0003: Real time only where waiting breaks something

| | |
| :--- | :--- |
| **System** | Baton (AI Delivery Sync & Feed for GitHub and Microsoft Teams) |
| **Status** | Accepted |
| **Date** | 2026-10-24 |
| **Deciders** | Ahmed Alhallag (AI architect) |
| **Principles** | [P3](../docs/architecture-principles.md#p3-quiet-by-default) |

## Context

Different events need different speeds. A prod deploy waiting for approval can't wait an hour. Four small merged PRs are better as one grouped post. A meeting transcript only exists after the meeting ends.

## Options considered

| Option | Good | Bad |
| :--- | :--- | :--- |
| 1. Everything in real time | Simple to explain | Noisy, more model calls, more cost |
| 2. Everything in batches | Cheap and calm | Urgent things wait too long |
| **3. By kind of event** | Fast where it matters, calm everywhere else | Two lanes to run |

## Decision

**Option 3.**

| What | When | Why |
| :--- | :--- | :--- |
| Pings ("needs you") | Within 1 minute | A prod approval or a stuck agent can't wait |
| Forwarded message → suggestion | Within 2 minutes | You're still thinking about it |
| Feed posts | Grouped every 10 minutes | 4 merged PRs become 1 post |
| Meeting → suggestions | Within 15 minutes of the meeting ending | The transcript only exists afterwards |
| Daily diff | 08:00 on working days | One calm summary |
| Board index | On every board event, plus a full re-sync nightly | Catches missed webhooks |

## Consequences

* ✅ Fewer model calls, because grouping cuts cost; a calmer feed.
* ⚠️ Two lanes to operate. *Mitigation:* one event queue with two consumers, an instant lane and a batch lane.

## Revisit when

Users ask for faster posts, or pings regularly hit the daily cap.
