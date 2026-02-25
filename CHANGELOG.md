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

### v1.0 — Initial Brainstorm (Day 1)
- Brainstormed 5+ skill ideas with Claude Sonnet 4.6: KOL scoring, social listening, competitor tracker
- Chose KOL Qualifier because it matched a real BD pain point: spending 3-4 hours manually evaluating KOLs per campaign
- First spec draft: basic 3-dimension scoring (Relevance, Engagement, Language)

### v1.1–v1.2 — Scoring Framework Design (Day 1)
- Added 4th dimension: **Content Style Fit** (30 pts) — realized topic relevance alone doesn’t capture tone/voice match
- Established weight philosophy: **content-first, reach-second** (70% for content dimensions, 30% for reach/language)
- Designed tier thresholds: tested 70, 75, 80 cutoffs with mock data → settled on 75 (A) / 50 (B) as sweet spot
- Added Readiness Gate concept: prevent AI from scoring garbage input

### v1.3 — Guardrails & Edge Cases (Day 2)
- Added 9 Guardrails (G1–G9) as non-negotiable rules after Claude self-critique found 6 weaknesses
- Designed bullet cap rule: 0–1 bullet → Relevance + Content Style Fit combined ≤ 20 pts → Tier A ceiling impossible
- Added duplicate detection (G8) after testing revealed mock data had duplicate @Domahhhh
- Wrote DM template structure: DM1 (opinion ask, zero pitch) + Soft CTA (separate) + Follow-up (Tier A only)

### v1.4 — Independent Review Round 1 (Day 2–3)
- Sent spec to Gemini 3.1 Pro (via Perplexity) for cold-read review — first time a different model saw the spec
- Gemini found 8 issues Claude missed: unclear STOP conditions, missing edge case documentation, vague scoring bands
- Applied 7 patches via GPT-5.2 Thinking; **rejected CAT-2-02** (sub-scoring within bands) as over-engineering for MVP

### v1.5 — SKILL.md Creation + Review Round 2 (Day 3)
- Wrote SKILL.md v1.0 as standalone system prompt derived from spec
- Second independent review (Gemini) focused on SKILL.md vs spec alignment
- Removed Vietnamese language support — MVP scope cut (see Things Tried and Abandoned below)
- Added Clarifying Questions gate (Q1–Q3) operating independently from Readiness Gate

### v1.6.0 — Final Release (Day 4)
- Added YAML frontmatter to SKILL.md per BGK feedback
- Added Self-Check (SC-1 through SC-7) — AI validates own output silently before presenting
- Enforced char limits: DM1 ≤ 300 chars, Follow-up ≤ 200 chars with explicit trim order
- Completed cross-model testing (3 runs, 2 campaigns, 2 AI models)
- Created AGENTS.md with 9 design decisions + 4 things tried and abandoned
- Finalized all deliverables: README, skill-card, CHANGELOG, ai-showcase/

---

## [Initial Setup] — Repo & Mock Data

- Created mock campaign: Whales Prediction Waitlist Launch (prediction markets domain)
- Created mock KOL dataset: 12 KOLs with intentional edge cases (duplicate handle, 0 followers, 0 bullets, CN-language KOLs, off-niche content)
- Created Skill Card (one-page summary with BEFORE/AFTER metrics)
- Created AI Showcase: 8 screenshots documenting the full multi-AI development process
- Set up README with Scoring Rubric summary, Files table, AI Pipeline Used, Edge Cases Demo, and How to Run guide

## Things Tried and Abandoned

### ❌ Vietnamese language support
**Problem:** Vietnam is a growing crypto market — should skill support VN outreach?
**Tried:** Early spec drafts included VN as a `target_market_language` option alongside EN and CN.
**Learned:** Adding VN would require separate test data, edge case handling, and DM template validation. No clear business demand in the initial use case — BD team’s active campaigns are all EN/CN.
**Decision:** Removed in v1.5. MVP focuses on EN/CN/mixed. VN can be added post-MVP with minimal spec changes.

### ❌ Gradient Language Match scoring (partial credit)
**Problem:** Should a CN-posting KOL get partial credit when the campaign targets EN?
**Tried:** Considered 10/7/3/0 gradient (exact match / partial / tangential / mismatch).
**Learned:** Language either matches or it doesn’t in outreach context. “Partial credit” implies some mismatches are acceptable — they’re not. The `mixed` campaign type already handles bilingual KOLs.
**Decision:** Kept binary 10/0. Documented as Design Decision #8 in AGENTS.md.

### ❌ Auto-crawl KOL data from X/Twitter
**Problem:** Manual paste is friction — why not crawl automatically?
**Tried:** Considered building Twitter API integration into the skill.
**Learned:** Crawling is a **discovery** problem; this skill solves **qualification**. Mixing both = scope creep. API dependencies (rate limits, auth tokens, data freshness) would make the skill fragile. Manual paste keeps it platform-agnostic.
**Decision:** Deliberate MVP cut. Phase 2 roadmap plans PhantomBuster/Chrome extension for auto-pull.

### ❌ avg_impressions_per_10_posts as scoring dimension
**Problem:** Follower count is a vanity metric — per-tweet engagement is more meaningful.
**Tried:** Spec included `avg_impressions_per_10_posts` in the KOL input schema.
**Learned:** Impression data requires either browser automation or manual lookup per KOL — too much friction at MVP. The follower bracket (<10k/10–100k/100k+) captures enough engagement signal for initial tier assignments.
**Decision:** Deferred to Phase 3. Bracket-based Engagement scoring is the pragmatic MVP choice.

### ❌ Larger scope: KOL discovery + qualification in one skill
**Problem:** Should the skill also help BD **find** KOLs, not just score them?
**Tried:** Early brainstorm included search/discovery features.
**Learned:** One skill doing two things = neither well. BD teams already have KOL lists from CRM, spreadsheets, or manual research. The real pain is **evaluation + outreach**, not discovery.
**Decision:** Scoped to qualification + DM drafting only. Discovery is a separate problem for a separate tool.

See `AGENTS.md` Part 3 for additional design decision rationale.

---

*CHANGELOG.md | Cook A Skill — Day 4 | By Sarah*
