# CHANGELOG.md — KOL Qualifier & Outreach Drafter

All notable changes to this skill are documented below.  
Only changes with evidence in the commit history or current files are included.

---

## [2026-02-24] — Spec v1.6.0 + SKILL.md v1.1 (Final Release)

**spec.md v1.6.0 (Final)**
- Completed 2 rounds of independent review (Gemini 3.1 Pro + GPT-5.2)
- Applied patches from review findings; rejected CAT-2-02 (sub-scoring within bands — over-engineering for MVP)
- Added Acceptance Tests (5 test cases: happy path, missing bullets, STOP rules, missing offer/cta, mixed language)
- Added Edge Cases section (language mismatch, off-topic bullets, duplicate handles)
- Finalized Demo Plan with 8-step breakdown and time allocation

**SKILL.md v1.1**
- Added YAML frontmatter (`name`, `description`, `version`) per BGK feedback
- Added Self-Check section (SC-1 through SC-7) — AI validates its own output before presenting
- Removed Vietnamese language from `target_market_language` — MVP focuses on EN/CN/mixed
- Enforced DM1 ≤ 300 chars and Follow-up ≤ 200 chars with explicit trim order

**Cross-Model Testing**
- 3 dry-runs: Run 1 (Gemini 3.1 Pro, Campaign 1), Run 2 (Claude Sonnet 4.6, Campaign 1), Run 3 (Claude Opus 4.6, Campaign 2 — DeFi domain)
- All runs produced consistent tier assignments; guardrails held across models
- Results saved in `test-results/`

---

## Spec Development & Review Cycle (pre-v1.6.0)

- Initial spec draft via Claude Sonnet 4.6 brainstorming session
- Defined 4-dimension scoring rubric: Relevance (40) + Content Style Fit (30) + Engagement (20) + Language Match (10)
- Designed Readiness Gate (0–10 scale) with 6 STOP conditions
- Added Clarifying Questions gate (Q1–Q3) — operates independently from Readiness Gate
- Established 9 Guardrails (G1–G9) as non-negotiable rules
- Built bullet cap rule: 0–1 bullet → Relevance + Content Style Fit combined ≤ 20 pts → Tier A ceiling impossible
- Wrote DM template structure: DM1 (opinion ask) + Soft CTA (separate block) + Follow-up (Tier A only)
- Version iterations driven by Claude self-critique (6 weaknesses found) + Gemini independent review

---

## [Initial Setup] — Repo & Mock Data

- Created mock campaign: Whales Prediction Waitlist Launch (prediction markets domain)
- Created mock KOL dataset: 12 KOLs with intentional edge cases (duplicate handle, 0 followers, 0 bullets, CN-language KOLs, off-niche content)
- Created Skill Card (one-page summary with BEFORE/AFTER metrics)
- Created AI Showcase: 8 screenshots documenting the full multi-AI development process
- Set up README with Scoring Rubric summary, Files table, AI Pipeline Used, Edge Cases Demo, and How to Run guide

## Things Tried and Abandoned

- **Vietnamese language support** → removed, MVP focuses on EN/CN/mixed
- **Multi-campaign batch comparison** → one campaign per run keeps output actionable
- **Auto-crawl KOL data** → deliberate separation of discovery vs qualification
- **avg_impressions_per_10_posts** → deferred to Phase 2 (too much data friction for MVP)

See `AGENTS.md` Part 3 for full rationale on each item.

---

*CHANGELOG.md | Cook A Skill — Day 4 | By Sarah*
