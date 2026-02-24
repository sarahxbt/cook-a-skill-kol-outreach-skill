# KOL Qualifier & Outreach Drafter

AI Skill for BD/Marketing: paste a campaign spec + KOL list → AI scores across 4 dimensions → assigns Tier A/B/C → drafts personalized DMs.

## Scoring Rubric

| Dimension | Weight |
|---|---|
| Relevance | 40 pts |
| Content Style Fit | 30 pts |
| Engagement | 20 pts |
| Language Match | 10 pts |

**Tier A** ≥ 75 → DM1 + Soft CTA + Follow-up  
**Tier B** 50–74 → DM1 + Soft CTA  
**Tier C** < 50 → No DM

## Files

| File | Description |
|---|---|
| `SKILL.md` | System Prompt v1.1 — paste into Claude Project Instructions |
| `spec.md` | Spec v1.6.0 — passed 2 rounds of independent review |
| `mock_campaign.md` | Campaign demo: Whales Prediction Waitlist Launch |
| `mock_kols.md` | 12 KOL test data with edge cases |
| `skill-card.md` | Skill Card (one-page summary) |
| `AGENTS.md` | AI models used, design decisions, things tried and abandoned |
| `CHANGELOG.md` | Version history with evidence from commit log |

## AI Showcase

The `ai-showcase/` folder contains 8 screenshots documenting the AI-assisted development process, in chronological order:

1. `01-ideation-brainstorm-claude.png` — 🧠 Brainstorming skill ideas with Claude (Sonnet 4.6)
2. `02-self-critique-prompt-weaknesses.png` — 🔍 Claude self-critiquing, identifying 6 weaknesses in the prompt
3. `03-multi-ai-review-pipeline.png` — 🔄 Writing a review prompt for another AI to cold-read (multi-AI pipeline)
4. `04-human-in-loop-selective-apply.png` — 👤 Human-in-the-loop: applied 7 findings, rejected CAT-2-02
5. `05-spec-finalized-build-skill.png` — 🏗️ Spec finalized (removed VN language), began writing SKILL.md
6. `06-compliance-audit-checklist.png` — 📋 AI auditing all GUIDEBOOK requirements (PASS/FAIL/PARTIAL)
7. `07-ai-automated-dry-run.png` — 🤖 AI running an automated dry-run (no manual Claude session needed)
8. `08-final-all-guardrails-pass.png` — ✅ Dry-run result: all guardrails PASS, scores match 100%

## AI Pipeline Used

| Step | AI Model | Role |
|:---|:---|:---|
| Brainstorm & Spec Draft | Claude Sonnet 4.6 (Claude Desktop) | Creative ideation, rapid iteration on spec structure |
| Self-Critique & Review Prompt | Claude Sonnet 4.6 (Claude Desktop) | Identified 6 weaknesses in own prompt; wrote review prompts for other AIs |
| Independent spec.md Review | Gemini 3.1 Pro (via Perplexity) | Cold-read review with full context — found blind spots Claude missed |
| Apply Review Patches | GPT-5.2 Thinking (via Perplexity) | Precise, targeted execution of fixes from review findings |
| Independent SKILL.md Review | Gemini 3.1 Pro (via Perplexity) | Second independent review on the system prompt |
| Final Verification | Claude Opus 4.6 (Antigravity IDE) | Deep compliance check against GUIDEBOOK requirements |
| Dry-run & Polish | Gemini 3.1 Pro + Claude Sonnet 4.6 + Claude Opus 4.6 (Antigravity IDE) | End-to-end test simulation + fix loop |

**Human-in-the-loop:** Every AI suggestion was reviewed manually. Findings that conflicted with design intent were rejected (e.g., rejected the rule "follower=0 → INVALID" because the existing bracket < 10k already handles it correctly at 4 pts).

**Key insight:** Each AI has a distinct role — Sonnet for creative ideation and fast execution, GPT for precise targeted fixes, Gemini for large-context cold reviews without bias, Opus for deep final verification. This is a **multi-model QA pipeline**, not a single-AI workflow.

## How to Run

1. Create a new Claude Project
2. Paste the entire `SKILL.md` into **Project Instructions**
3. In the chat, paste `mock_campaign.md` + `mock_kols.md`
4. Answer the 3 Clarifying Questions → the Skill runs automatically

## Edge Cases Demo

- **Duplicate KOL** → Skill halts, prompts user for confirmation
- **0 bullets** → Guardrail caps Relevance + Content ≤ 20, tags `[GENERIC]`
- **Language mismatch** → Language Match = 0
- **Off-topic content** → Relevance falls to None/Tangential band
- **Follower = 0 or very low** → Bracket < 10k = 4 pts (valid, NOT invalid)

---

*Cook A Skill — Day 4 | By Sarah*