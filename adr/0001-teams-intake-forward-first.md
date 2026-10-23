# ADR-0001: Read Teams by forwarding, not by listening

| | |
| :--- | :--- |
| **System** | Baton (AI Delivery Sync & Feed for GitHub and Microsoft Teams) |
| **Status** | Accepted |
| **Date** | 2026-10-24 |
| **Deciders** | Ahmed Alhallag (AI architect) |
| **Principles** | [P1](../docs/architecture-principles.md#p1-ai-suggests-you-approve-code-applies), [P4](../docs/architecture-principles.md#p4-messages-are-data-not-instructions), [P5](../docs/architecture-principles.md#p5-private-stays-private) |

## Context

Most delivery decisions happen in Microsoft Teams, so the system needs that context to suggest board updates. Microsoft Graph can read chats, channel messages and meeting transcripts, but:

* reading chats across the tenant needs admin consent, and some of those APIs are protected and need Microsoft's approval;
* Teams chat notifications and transcript downloads are metered and billed through Azure;
* admins can switch off API access to transcripts;
* chats hold client details and personal talk, while GitHub issues are visible to the whole organisation.

## Decision drivers

* Privacy and the team's trust (P5)
* The smallest set of permissions an admin can approve
* High signal: content a person chose to share is more likely to be real work
* Cost

## Options considered

| Option | How it works | Good | Bad |
| :--- | :--- | :--- | :--- |
| **1. You forward** | A "Send to Baton" action on any Teams message, plus transcripts of meetings you organise | Minimal permissions, clear consent, high signal, cheap | Misses what you don't forward |
| 2. Listen to everything | Tenant-wide Graph subscriptions to all chats and channels | Nothing is missed | Protected and metered APIs, broad consent, surveillance feel, noisy |
| 3. Selected channels | A bot added to chosen project channels (resource-specific consent) | Team-level coverage with a clear scope | More setup; still needs the team's agreement |

## Decision

**Option 1 for v1.** Option 3 follows in v3 for project channels, once the team agrees and the suggestions have earned trust. Option 2 is ruled out.

## Consequences

* ✅ Privacy by design: every message has a person who chose to share it.
* ✅ The permissions are small enough for an admin to approve quickly.
* ⚠️ It misses what isn't forwarded. *Mitigation:* the action is one click, and the daily diff lists meetings with no transcript shared.
* ⚠️ One more click for the user. *Mitigation:* v3 adds selected channels.

## Revisit when

The team asks for channel coverage, or forwarding goes above 40 messages a day.
