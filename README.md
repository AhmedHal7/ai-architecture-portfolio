<div align="center">

# AI Architecture Portfolio

**Ahmed Alhallag** · [AI Architecture Design](https://academy.mlopsmena.com/en/courses/ai-architecture-design) · MLOps MENA · Oct–Dec 2026

![Course](https://img.shields.io/badge/course-AI%20Architecture%20Design-4F46E5?style=flat-square)
![Session 1](https://img.shields.io/badge/session%201-governance%20pack-0D9488?style=flat-square)
![Capstone](https://img.shields.io/badge/capstone-part%201%20draft-F59E0B?style=flat-square)

</div>

---

## 🏃 Featured system: Baton (AI Delivery Sync & Feed for GitHub and Microsoft Teams)

> **Relay races are lost at the handoff, not on the straight. So is software delivery.**

Delivery work lives in three places that drift apart. It is **talked about** in Microsoft Teams, **tracked** on a GitHub Projects board, and more and more of it is **done** by AI coding agents (Claude Code, GitHub Copilot). Someone has to be the glue between them, and it is usually the person running the programme.

**Baton (AI Delivery Sync & Feed)** is that glue. It watches the handoffs between people, agents and tools, and turns them into one calm feed with three kinds of card:

| Card | What it is | Example |
|---|---|---|
| 🔴 **Needs you** | The rare ping. Only things that wait on a human. | "Release 2.4 passed pre-prod. The prod deploy is waiting for your approval." |
| 💡 **Suggestion** | Something said in Teams that should change the board. | "Lina's message is about #412, closed 3 days ago. Reopen it?" |
| 📰 **News** | What happened, in one sentence. | "Claude opened a PR for the webhook retry. Tests pass." |

**One rule runs through everything:** the AI suggests, a human approves, and plain code applies the change. Nothing is written to GitHub without a click.

### How it works

[![Baton (AI Delivery Sync & Feed) architecture flow](diagrams/baton-architecture-flow.png)](diagrams/baton-architecture-flow.png)

① **Collect** GitHub events, agent sessions, and the Teams messages and meetings you share → ② **Sort** noise from news → ③ **Match** each message to the right board item → ④ **Write** short posts and suggestion cards → ⑤ **You decide** → ⑥ **Apply** with the only write token.

### Who it's for

Baton (AI Delivery Sync & Feed) starts with the programme lead (product manager, architect, engineering lead) and grows into a tool for the **whole team**. Each person gets a **personalized feed**: the products and workstreams they follow, their own PRs and reviews, mentions of them, and their agents' sessions, with their own ping rules. The details come in later sessions; see the [roadmap](#️-roadmap).

## 📚 Deliverables

| Deliverable | What's inside | Link |
|---|---|---|
| **Governance pack** (Session 1 mini project) | Goals, principles, ADRs, ARB gate, architecture flow and risks on one page | [docs/governance-pack-wiki.md](docs/governance-pack-wiki.md) |
| Architecture principles | Six principles, each tied to a business goal | [docs/architecture-principles.md](docs/architecture-principles.md) |
| Architecture decision records | How Teams is read, where models run, real time vs batch, how matching works | [adr/](adr/) |
| ARB checklist | The release gate: security, privacy, evals, cost, human oversight | [docs/arb-checklist.md](docs/arb-checklist.md) |
| **Capstone part 1** (framing) | Scenario, problem, goals, stakeholders, ranked quality attributes, scope, C4 levels 1 and 2 | [docs/01-framing.md](docs/01-framing.md) |
| Diagrams | PNG exports plus editable draw.io sources | [diagrams/](diagrams/) |

## 🗂️ Repository structure

```text
ai-architecture-portfolio/
├── README.md                          ← you are here
├── docs/
│   ├── governance-pack-wiki.md        ← Session 1 deliverable (start here)
│   ├── 01-framing.md                  ← Capstone part 1: problem, goals, quality attributes, C4
│   ├── architecture-principles.md
│   └── arb-checklist.md
├── adr/
│   ├── 0001-teams-intake-forward-first.md
│   ├── 0002-llm-hosting-managed-in-region.md
│   ├── 0003-real-time-vs-batch.md
│   └── 0004-matching-refs-first-hybrid-search.md
├── diagrams/
│   ├── baton-architecture-flow.png
│   ├── c4-l1-context.png
│   ├── c4-l2-containers.png
│   └── src/                           ← editable .drawio sources
└── capstone/                          ← final architecture, slides and launch kit
```

## 🗺️ Roadmap

| Stage | Who | What |
|---|---|---|
| v1 | Programme lead | Feed, pings and daily diff; suggestions from forwarded Teams messages and meetings you run |
| v2 | Whole team | A personalized feed and ping rules for each person: "my PRs, my reviews, my mentions, my agents" |
| v3 | Team and agents | Selected Teams channels, with the team's consent; hand a small task to a coding agent straight from a suggestion card |

## 📅 Progress

| Session | Date | Work | Status |
|---|---|---|---|
| 1 – Framing | 18 Oct | Governance pack (due 28 Oct) and capstone framing | ✅ Draft |
| 2 – AI building blocks | 25 Oct | Model choices and cost per suggestion | ⏳ |
| 3 – LLM & RAG | 1 Nov | Matching design and the eval set | ⏳ |
| Capstone part 1 | 14 Nov | Scenario, problem, quality attributes, C4 levels 1 and 2 | ✅ Draft |
| 4 – Agents | 15 Nov | The suggestion agent and hand-offs to coding agents | ⏳ |
| 5 – Security, governance, evals | 22 Nov | Injection tests, privacy review, eval results | ⏳ |
| 6 – Case studies | 29 Nov | Build vs buy | ⏳ |
| Final capstone | 13 Dec | `architecture.pdf` and `slides.pdf` | ⏳ |

## ℹ️ About the scenario

Bunyan Digital and its Atlas programme are **fictional**. The scenario is modelled on the author's day-to-day work running an AI-assisted delivery programme on GitHub Enterprise and Microsoft Teams. All names, numbers and data are illustrative, and Baton (AI Delivery Sync & Feed) is a course concept, not a released product.
