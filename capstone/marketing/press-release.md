# 📰 Press release (working backwards) — Baton (AI Delivery Sync & Feed)

> **What this is:** an Amazon-style *working backwards* press release, written **before** anything is built, to test whether the idea is worth building. Bunyan Digital, its people and its numbers are fictional. The launch date is a placeholder.

---

## Bunyan Digital introduces Baton (AI Delivery Sync & Feed), so delivery teams never drop the baton between Teams, GitHub and their AI agents

**A calm feed that keeps Microsoft Teams conversations, the GitHub board and AI coding agents in sync, and only pings you when something is waiting on you.**

**RIYADH — [LAUNCH DATE]** — Bunyan Digital today introduced Baton (AI Delivery Sync & Feed) for teams that plan in GitHub, talk in Microsoft Teams and ship with AI coding agents such as Claude Code and GitHub Copilot.

Modern delivery work lives in three places that drift apart. Decisions are made in meetings and chats, the plan lives on a GitHub board, and more and more of the work is done by AI agents that open pull requests and then wait. Someone has to be the glue: usually the person running the programme, copying meeting outcomes onto the board by hand and checking GitHub again and again. Things get lost at the handoffs. An action agreed in a meeting never reaches the board, an agent's pull request waits two days for a reviewer, and a release sits in pre-prod because nobody noticed it was ready.

Baton (AI Delivery Sync & Feed) watches those handoffs. It turns GitHub events and agent sessions into one feed written in plain sentences. When you share a Teams message or a meeting, it finds the board item the conversation is about and suggests what to do: link it, update it, add a sub-issue, merge a duplicate or create something new. You approve with one click, and nothing is ever written to GitHub without you. It pings you only when something is truly waiting on you, five times a day at most, and sends a short daily diff every morning.

*"I used to be the human sync service between our meetings and our board,"* said the programme architect of Bunyan's Atlas programme (a fictional persona). *"Now the meeting ends and six suggestions are waiting. I approve four, ignore two, and the board is right before I've had my coffee."*

Baton (AI Delivery Sync & Feed) was designed for trust from the start. It reads only the messages you forward and the meetings you run, never every chat. The AI suggests; a person approves; plain code applies the change with a single, tightly scoped write token. Status, owners and dates are read from GitHub, never made up, and every post links back to its source. Model calls stay in-region with zero data retention.

It starts with the programme lead and grows into a tool for the whole team, with a personalized feed for every teammate: their pull requests, their reviews, their mentions and their agents.

*"Before, I found out a release was blocked on me from someone asking 'any update?' in a chat,"* said a QA engineer on the Atlas team (a fictional persona). *"Now it's in my feed before anyone has to ask."*

To try the concept, read the architecture at **github.com/AhmedHal7/ai-architecture-portfolio**.

---

## FAQ

**Does it read all my Teams chats?**
No. In v1 it reads only the messages you send to it ("Send to Baton") and the transcripts of meetings you run. Selected project channels may come later, with the team's agreement. See [ADR-0001](../../adr/0001-teams-intake-forward-first.md).

**Can it change GitHub on its own?**
No. The AI only suggests. A person approves every change, and a small separate service, the only holder of a write token, applies it. It can't delete, merge or deploy. See [P1](../../docs/architecture-principles.md#p1-ai-suggests-you-approve-code-applies).

**How is this different from GitHub Agent HQ or the GitHub app for Teams?**
Agent HQ helps you run and steer coding agents, and the GitHub app for Teams lets you ask Copilot to do something. Baton (AI Delivery Sync & Feed) does neither. It notices what was said, checks it against the board, suggests updates, and tells the programme's story in one calm feed. It builds on their pieces rather than competing with them.

**Is it only for managers?**
No. v1 serves the programme lead. v2 gives every teammate a personalized feed and their own ping rules. It never shows per-person speed or rankings ([P6](../../docs/architecture-principles.md#p6-work-not-people-scores)).

**What does it cost to run?**
[TO BE ESTIMATED IN SESSION 2.] The target is under 5 cents per suggestion, with a monthly budget and an alert at 80%.

**What could go wrong?**
Wrong suggestions, noise and privacy leaks. Each has a guardrail: approval before any write, a daily ping cap, summary-plus-link instead of private text, and an eval set with pass marks before launch. See the [ARB checklist](../../docs/arb-checklist.md).
