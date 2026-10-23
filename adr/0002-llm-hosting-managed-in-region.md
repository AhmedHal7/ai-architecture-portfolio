# ADR-0002: Run the AI models as a managed service in our cloud tenant, in-region

| | |
| :--- | :--- |
| **System** | Baton (AI Delivery Sync & Feed for GitHub and Microsoft Teams) |
| **Status** | Accepted |
| **Date** | 2026-10-24 |
| **Deciders** | Ahmed Alhallag (AI architect) |
| **Principles** | [P5](../docs/architecture-principles.md#p5-private-stays-private), [P2](../docs/architecture-principles.md#p2-facts-come-from-the-source) |

## Context

The system sends Teams messages, meeting transcripts and issue text to language models to sort, match and write. That text can hold client details. Clients expect their data to stay in the country (Saudi PDPL in the scenario). The work needs two kinds of model: a cheap, fast one to sort events, and a stronger one to match messages to the board and write clearly.

## Decision drivers

* Data stays in-region, and nothing is used for training
* Quality of matching and writing in English and Arabic
* No GPUs to run
* Cost per suggestion

## Options considered

| Option | Good | Bad |
| :--- | :--- | :--- |
| 1. A vendor's public API directly | Newest models first, simple | Data leaves our tenant; the region may not match |
| **2. Managed models in our cloud tenant** (for example Microsoft Foundry or Amazon Bedrock) | Data stays in our tenant and region, enterprise terms, one bill | Model choice per region can lag behind |
| 3. Self-hosted open models on GPUs | Full control | GPUs to buy and run, and weaker quality for this effort |

## Decision

**Option 2**, in two tiers: a small model to sort events (②) and a stronger model to match and write (③④). Zero data retention is required in the contract. All calls go through a thin model gateway, so the model can be swapped without code changes.

## Consequences

* ✅ Data stays in-region, under enterprise terms.
* ✅ The small model handles most events, which keeps cost down.
* ⚠️ The best model may not be in our region yet. *Mitigation:* the gateway plus the eval set make a switch a measured decision.
* ⚠️ Vendor dependency. *Mitigation:* prompts and evals are model-neutral.

## Revisit when

The model we need isn't available in-region, or the monthly model cost goes above budget for two months in a row.
