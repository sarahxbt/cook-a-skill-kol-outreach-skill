# AGENTS.md — KOL Qualifier & Outreach Drafter

---

## Part 1 — AI Agents Used

| # | AI Model | Task | Why this model? |
|---|---|---|---|
| 1 | Claude Sonnet 4.6 (Claude Desktop) | Brainstorm ideas, write first spec draft | Fast creative ideation with strong context retention — Sonnet excels at generating multiple concepts quickly and structuring a first draft from a vague brief. |
| 2 | Claude Sonnet 4.6 (Claude Desktop) | Write review prompts for other AIs to run (prompt engineering) | Same model to maintain consistency — Sonnet understands its own blind spots well enough to write review prompts that target them. Self-critique identified 6 weaknesses before external review. |
| 3 | Gemini 3.1 Pro (via Perplexity web UI) | Independent cold review of spec.md — no bias from Claude | Large context window for ingesting the entire spec + guidebook in one shot. Zero exposure to earlier Claude conversations = genuinely independent review. Found blind spots Claude missed. |
| 4 | GPT-5.2 Thinking (via Perplexity web UI) | Apply precise patches from review findings | GPT Thinking mode excels at precise, surgical edits — it follows patch instructions literally without over-correcting or rewriting surrounding context. Best for "change X to Y" tasks. |
| 5 | Claude Sonnet 4.6 (Claude Desktop) | Re-review after GPT applied patches | Sonnet re-reads the patched version fresh, verifying that GPT's fixes didn't introduce new inconsistencies. Familiar with original design intent → catches semantic drift. |
| 6 | Gemini 3.1 Pro (via Perplexity web UI) | Second independent review of SKILL.md | Second independent review using the same unbiased model. Ensures the system prompt (SKILL.md) aligns with the spec — different artifact, same standard of scrutiny. |
| 7 | Claude Opus 4.6 (Antigravity IDE) | Final verification + GUIDEBOOK compliance check | Opus is the highest-reasoning Claude model — ideal for deep compliance auditing where every guardrail must be verified against the competition guidebook. Higher accuracy on subtle logical gaps. |
| 8 | Gemini 3.1 Pro + Claude Sonnet 4.6 + Claude Opus 4.6 (Antigravity IDE) | 3 dry-runs cross-model + cross-domain test | Three different models running the same skill with the same data = cross-model consistency test. If all produce the same tier assignments, the scoring rubric is robust. Also tested a second campaign domain (DeFi) to prove portability. |

> **Human-in-the-loop:** I reviewed every AI suggestion and rejected those that conflicted with design intent. Example: rejected the recommendation to flag `follower=0` as `INVALID` — because the bracket < 10k already handles it correctly at 4 pts. AI assists the process; I own the decisions.

---

## Part 2 — Design Decisions

### 1. Weight 40/30/20/10 — not equal distribution
**Decision:** Relevance gets the highest weight (40), followed by Content Style Fit (30), Engagement (20), Language Match (10).  
**Why:** Content-first, reach-second philosophy. A KOL with 5k followers who writes deeply about our exact topic is more valuable than a 500k-follower generalist who never engages with our niche. The 40/30 split on content dimensions (70% total) ensures that what the KOL actually talks about matters more than vanity metrics. This reflects how real outreach works — a personalized DM based on relevant content gets replies; a generic DM to a big account doesn't.

### 2. Tier A threshold ≥ 75 — not 80 or 70
**Decision:** KOLs scoring 75+ are Tier A (priority outreach with full DM package).  
**Why:** 80 would be too restrictive — only perfect-fit KOLs with large followings and matching language would qualify, leaving an empty pipeline. 70 would be too loose — KOLs with partial topic alignment would get full outreach treatment, wasting BD time on low-probability replies. 75 is the sweet spot: strong content alignment is required, but a language mismatch or smaller following alone doesn't disqualify an otherwise excellent fit.

### 3. DM1 ≤ 300 characters — not 500
**Decision:** First outreach DM is hard-capped at 300 characters.  
**Why:** Mobile DM preview on X/Twitter shows approximately the first 280-300 characters. If the hook and personalization aren't visible in the preview, the KOL won't tap to read more. 500 characters means the KOL sees a wall of text and ignores it. The constraint forces every DM to lead with the most compelling personalized hook — no wasted pleasantries. This is a business decision, not a technical one.

### 4. Readiness Gate threshold ≥ 6 — not 5 or 7
**Decision:** Campaign must score at least 6/10 on the Readiness Gate before the skill runs.  
**Why:** The gate scores 5 conditions: full spec (+3), key_topics ≥ 2 (+2), ≥ 1 KOL with bullets (+2), tone+goal present (+1), ≥ 5 KOLs (+2). At threshold 6, a campaign must hit at least 3 of 5 conditions — enough signal that AI scoring is meaningful. At 5, a team could pass with just a complete spec (+3) and key_topics (+2) but zero KOL bullets — meaning every DM would be [GENERIC]. At 7, a solid campaign missing just one minor condition (e.g., only 4 KOLs instead of 5) gets blocked unnecessarily.

### 5. follower=0 is valid — not flagged INVALID
**Decision:** A KOL with 0 followers is scored normally in the < 10k bracket (4 pts Engagement).  
**Why:** `INVALID` is reserved for unparseable follower counts (e.g., "many", "N/A", blank text). Zero followers is a real, parseable number — it means the account exists but is new or niche. The bracket system already handles it: < 10k = 4 pts, which appropriately discounts reach without rejecting the KOL entirely. A new account writing deeply relevant content can still score well on Relevance and Content Fit. Flagging 0 as INVALID would incorrectly penalize legitimate edge cases.

### 6. campaign_tone has 3 components — not 1 keyword
**Decision:** `campaign_tone` is written as a multi-faceted phrase (e.g., "builder-to-builder, hyper-analytical, insider-insight") rather than a single word like "professional" or "friendly".  
**Why:** A single keyword is too ambiguous for AI to score Content Style Fit accurately. "Professional" could mean corporate-formal, data-driven, or consultative — each implies different content patterns. Three components give the AI a triangulation point: the DM should sound like a builder (not a marketer), be data-heavy (not hype), and reference insider knowledge (not surface-level takes). This makes the 30-point Content Style Fit dimension actually meaningful instead of "vibes-based."

### 7. Rejected CAT-2-02 — sub-scoring within bands
**Decision:** Rejected the review suggestion to add deterministic sub-scoring within each Relevance band (e.g., Strong → 34/36/38/40 based on sub-criteria).  
**Why:** Over-engineering for MVP. The purpose of band-level scoring is to assign tiers for business decisions — and all KOLs in the same band produce the same action (same tier = same outreach template). Whether a Tier A KOL scores 34 or 38 doesn't change what BD does: they get DM1 + Soft CTA + Follow-up. Adding sub-criteria would triple the rubric complexity, increase AI processing time, and create false precision that doesn't improve the outreach outcome. Band consistency is sufficient; sub-point precision is a Phase 2 consideration.

### 8. Language Match is binary 10/0 — not gradient
**Decision:** Language Match scores either 10 (match) or 0 (mismatch). No partial credit.  
**Why:** Language either matches or it doesn't — there's no meaningful middle ground. A KOL who posts in Chinese when the campaign targets English speakers cannot receive a "partially matching" score. Gradient scoring would imply that some mismatches are less bad than others, which isn't how outreach works: you either write the DM in a language the KOL engages in, or you don't. The `mixed` campaign type already handles the nuance — if the campaign targets both EN and CN, both languages score 10.

### 9. Multi-AI pipeline — not single model
**Decision:** Used 5+ different AI models across 8 steps of the development workflow, rather than doing everything in one Claude session.  
**Why:** Each model has distinct strengths — Sonnet for fast creative drafting, GPT for precise surgical patches, Gemini for large-context independent reviews, Opus for deep compliance verification. More importantly, cross-model review eliminates single-model bias: if Claude writes the spec and Claude reviews the spec, blind spots survive both passes. By using Gemini as an independent cold reviewer (zero prior context), we caught issues that Claude's self-critique missed. The multi-AI approach is essentially a QA pipeline — each model serves a specific function, not a redundant duplicate.

---

## Part 3 — Things Tried and Abandoned

### ❌ Vietnamese language support
**What:** Early spec drafts included Vietnamese (VN) as a supported `target_market_language` option alongside EN and CN.  
**Why abandoned:** MVP focuses on EN/CN/mixed because those are the primary markets for crypto/Web3 KOL outreach. Adding VN would require additional test data, edge case handling, and DM template validation without clear business demand in the initial use case. Removed during spec finalization to keep the skill focused.

### ❌ Multi-campaign batch comparison
**What:** Considered allowing users to run multiple campaign specs at once and compare which KOLs overlap across campaigns.  
**Why abandoned:** Adds complexity without clear MVP value. Each campaign has different key_topics, tone, and language — comparing scores across campaigns is apples-to-oranges. One campaign per run keeps the output clean and actionable. Batch mode is scoped for post-MVP.

### ❌ Auto-crawl KOL data from X/Twitter
**What:** Considered building the skill to automatically fetch KOL profiles, follower counts, and recent tweets.  
**Why abandoned:** Deliberate MVP cut — separation of concerns. KOL *discovery* and *qualification* are different problems. This skill qualifies a list the user has already compiled. Auto-crawling introduces API dependencies, rate limits, and data freshness issues. The manual paste approach keeps the skill platform-agnostic and zero-dependency.

### ❌ avg_impressions_per_10_posts as a scoring dimension
**What:** Spec included an `avg_impressions_per_10_posts` field in the KOL input schema.  
**Why abandoned:** Deferred to Phase 3. Per-tweet engagement data (avg likes, replies, reposts) requires either browser automation or manual lookup per KOL — too much friction for MVP. The follower bracket captures enough engagement signal for initial tier assignment. Phase 3 will add per-tweet engagement as a 5th scoring dimension once data collection is automated in Phase 2.

---

*AGENTS.md | Cook A Skill — Day 4 | By Sarah*
