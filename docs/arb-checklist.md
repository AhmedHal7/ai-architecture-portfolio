# 📋 Architecture Review Board (ARB) Checklist — Baton (AI Delivery Sync & Feed)

The release gate for Baton (AI Delivery Sync & Feed for GitHub and Microsoft Teams). Every change goes through it: a new feature, a new model, a new prompt or a new data source. **Any "no" means it doesn't ship until it's fixed.**

* **Reviewer:** Ahmed Alhallag (AI architect)
* **Status:** draft
* **Governing principles:** [P1–P6](architecture-principles.md)
* **Governing ADRs:** [0001](../adr/0001-teams-intake-forward-first.md), [0002](../adr/0002-llm-hosting-managed-in-region.md), [0003](../adr/0003-real-time-vs-batch.md), [0004](../adr/0004-matching-refs-first-hybrid-search.md)

---

## 1. Security
- [ ] Does only the Applier hold a GitHub write token, limited to one board and to comment, link, set field, add sub-issue and create? (P1)
- [ ] Is it impossible for the system to delete, merge or deploy, because no token allows it?
- [ ] Is every incoming webhook, hook and forwarded message checked for a valid signature?
- [ ] Is text from Teams, transcripts, issues and PRs treated as data, with injection cases in the eval set? (P4)
- [ ] Are all secrets in a vault, none in code or in the web app?

## 2. Privacy
- [ ] Does it read only forwarded messages and meetings the user runs? (ADR-0001)
- [ ] Does private chat text stay out of org-visible issues, with only a summary and a link? (P5)
- [ ] Do all model calls and stored data stay in-region, with zero retention at the model provider? (ADR-0002)
- [ ] Are transcripts deleted after 30 days, and can a person ask for their messages to be removed?
- [ ] Do meeting participants know the transcript is shared with the system?

## 3. Evals and observability
- [ ] Is there a labelled set (at least 100 messages and 10 meetings) with pass marks agreed before launch?
- [ ] Is every model call traced (model version, tokens, time, cost)?
- [ ] Is "accepted without edits" tracked for each suggestion type?
- [ ] Is every ping checked for whether someone acted on it within an hour?

| Metric | Pass mark before launch |
| :--- | :--- |
| The right item is the top suggestion | ≥ 85% |
| "Create new" suggested when the item already exists | ≤ 5% |
| Pings acted on within 1 hour | ≥ 80% |
| Injection tests that trigger an action | 0 of 20 |
| Facts in posts that don't match GitHub | 0 of 200 sampled |

## 4. Cost
- [ ] Is there a monthly budget for model calls, with an alert at 80%?
- [ ] Does the small model filter out chatter before the stronger model is called?
- [ ] Is the cost per suggestion estimated and kept under 5 cents? (worked out in Session 2)
- [ ] Is real time used only where waiting breaks something? (ADR-0003)

## 5. Human oversight
- [ ] Is nothing written to GitHub without a human approval? (P1)
- [ ] Is there one Pause switch that stops every write and every ping at once?
- [ ] Does every card show why it was suggested (references, matched words, confidence)?
- [ ] Is every suggestion, choice and write kept in an append-only audit log, with one-click undo?
- [ ] Is it clear that the feed shows work, not people scores? (P6)
