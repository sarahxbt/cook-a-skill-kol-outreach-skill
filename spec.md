# spec.md — KOL Qualifier & Outreach Drafter
**Skill Name:** KOL Qualifier & Outreach Drafter
**Owner:** BD & Marketing
**Version:** v1.6.0 — Day 1 MVP (Final)
**Status:** Approved

---

## 1. Problem Statement

Every time the team prepares a GTM campaign, BD/Marketing must:
1. Manually scan KOL profiles one by one — read content, assess fit, take notes
2. Score inconsistently across team members → missed opportunities, no shared standard
3. Write outreach from scratch for each KOL → generic messages → low reply rates

**This skill solves all three steps in a single, repeatable workflow.**

---

## 2. User

| | |
|---|---|
| **Primary user** | BD/Marketing (skill author — live demo) |
| **Secondary user** | BD/Marketing team members using the skill after it's shared |

---

## 3. Assumptions & Unknowns

### Assumptions (locked for MVP)
- KOL discovery (finding new KOLs via web scanning) is out of scope; this skill qualifies a list the user has already compiled — separation of discovery from qualification is the deliberate MVP tradeoff.
- User always provides data manually via copy-paste; no API or web scraping
- `recent_content_bullets` are written by the user in their own words — not crawled
- Each KOL has at most 3 bullets; at minimum 0 (missing = explicitly flagged)
- Scoring is deterministic: same input → same tier every time
- DM1 is always sent before any collab mention; Soft CTA is always drafted as a separate output block for Tier A and B — it is conditionally sent only after KOL replies to DM1, but it is always pre-generated.
- Campaign spec is always provided in English, regardless of `target_market_language`
- One campaign spec per run; batch multi-campaign comparison is out of scope for MVP

### Unknowns (to resolve before Day 2)
- What is the expected quota per tier? (e.g., "need at least 5 Tier A") — affects how user interprets scorecard
- Does the team have a preferred DM platform? (X/Twitter DM, Telegram, email) — affects DM formatting
- Are there any KOLs that should always be excluded (competitor-adjacent, blacklisted)?
- Will the skill be run by a single person or multiple team members simultaneously?

---

## 4. Clarifying Questions (Mandatory Before Generating Output)

The skill **must ask all 3 questions and wait for answers before scoring or drafting any DM.** Do not generate output based on assumed answers.

```
Before I begin, I need 3 quick answers:

Q1 [Reply Direction & Next Step] — What exactly do you want the KOL to do in their first reply?
  (e.g., share a quick opinion, agree to a 10-min chat, record a quick take, give written feedback)
  This determines how the Soft CTA is written and what "success" looks like for DM1.

Q2 [Tone] — How should the DM feel?
  (e.g., peer-to-peer casual, respectful fan, researcher asking expert, industry insider)
  This determines the voice and framing of DM1.

Q3 [Hard Constraints] — Any non-negotiable filters?
  (e.g., "only KOLs who post in Vietnamese", "exclude anyone with < 10k followers",
  "no KOLs who have posted about competitor X")
  These will be applied before scoring begins.
```

> If the user skips any question, the skill must re-ask that specific question before proceeding. Partial answers are not accepted. A partial answer is defined as: the user acknowledges the question but provides insufficient specificity to act on (e.g., 'yes' or 'I don't know' without elaboration). A single-word directional answer (e.g., 'casual' for Q2) is accepted. A one-word or content-free answer (e.g., 'ok', 'whatever') must trigger a re-ask.

---

## 5. Input Schema

### 5a. Campaign Spec (`campaign-spec.md`)

```
campaign_name:
product_description:
target_audience:
value_proposition:
key_topics: []             # topics/narratives KOL should speak to
target_market_language:    # EN | CN | mixed
campaign_tone:             # e.g., educational, hype, community-first
campaign_goal:             # awareness | launch | trust | community
offer:  # what is being offered to the KOL (even if not mentioned in DM1)
cta:    # desired next action (e.g., "10-min chat", "quick take on product")
```

> If any field is missing → skill writes `MISSING — required before scoring` and halts that dimension. No inference from other fields.

---

### 5b. KOL List (copy-paste from spreadsheet)

User provides a list of KOLs formatted as a table (or CSV/spreadsheet copy-paste). The table **must** contain the following columns:

- **Handle**: e.g., @username
- **Follower**: numeric value (e.g., 00000)
- **avg_impressions_per_10_posts**: optional (Phase 2); leave blank in MVP
- **Niche/Tags**: e.g., tag1, tag2
- **Language**: EN | CN
- **Recent Content Bullets**: The user's notes on the KOL's recent posts. Minimum 0 (empty cell), maximum 3 bullets.

**Field rules:**
- `Niche/Tags` → used for Relevance scoring only; never used as evidence in DM copy
- `Language` → used for Language Match scoring. If missing, score as 0 pts and note 'missing language'.
- `Recent Content Bullets` → the only source of personalization in DMs. If more than 3 bullets are provided, use only the first 3 and ignore the remainder.
- `Follower` → used for Engagement scoring only. Must be evaluated as an integer. If the value contains commas, 'k' notation, or approximation symbols (e.g., '~'), the skill must normalize it first (100k → 100000). If the value cannot be parsed, flag as `[INVALID]` and score Engagement as 0.
- `avg_impressions_per_10_posts` → (optional, Phase 2) not used in MVP scoring
- Missing `Recent Content Bullets` (0 cells) or only 1 bullet → flag as `[generic/no evidence]`; cap Relevance + Content Style Fit combined ≤ 20 pts.

---

## 6. Scoring Rubric

Total: **100 points**

| Dimension | Weight | Scoring Basis | Data Source |
|---|---|---|---|
| **Relevance** | 40 pts | Do KOL's topics align with `key_topics`? | `niche_tags` + `recent_content_bullets` |
| **Content Style Fit** | 30 pts | Does KOL's tone/format match `campaign_tone`? | `recent_content_bullets` only |
| **Engagement** | 20 pts | Follower count bracket only | `follower_count` |
| **Language Match** | 10 pts | Does KOL's language match `target_market_language`? | `language` field |

**Relevance bands (40 pts):**
- Strong alignment to ≥ 2 `key_topics` (via bullets or niche_tags) = 34–40
- Partial alignment to 1 `key_topic` = 20–33
- Tangential / weak alignment = 10–19
- No alignment = 0–9

> **Scoring priority:** `Recent Content Bullets` are primary evidence — they dominate when present. `Niche/Tags` serve as secondary context only; they may contribute up to half the Relevance score when bullets are absent or insufficient.

**Content Style Fit bands (30 pts):**
- Tone/format clearly matches `campaign_tone` = 25–30
- Partially matches = 15–24
- Neutral / unclear = 8–14
- Mismatches = 0–7

**Engagement bracket (follower_count → pts):**

| Followers | Points |
|---|---|
| ≥ 500k | 20 |
| 100k–499k | 16 |
| 50k–99k | 12 |
| 10k–49k | 8 |
| < 10k | 4 |

> Engagement score is based **solely on follower_count bracket**. Do not infer posting frequency, interaction rate, or quality of comments from any other field. `avg_impressions_per_10_posts` is reserved for Phase 2.

> **Mixed Language Rule:** If `target_market_language = mixed`: KOL posting in either EN or CN scores 10 pts; KOL posting in neither scores 0 pts.

**Tier thresholds:**

| Tier | Score | Action |
|---|---|---|
| 🟢 **A** | ≥ 75 | Priority outreach — DM1 + Soft CTA + Follow-up DM |
| 🟡 **B** | 50–74 | Secondary outreach — DM1 + Soft CTA only |
| 🔴 **C** | < 50 | Skip this campaign |

**Guardrail cap:** If `recent_content_bullets` is missing or has only 1 bullet → Relevance + Content Style Fit combined maximum = 20 pts → KOL cannot reach Tier A regardless of other scores.

---

## 7. Readiness Gate

Before generating any output, the skill scores campaign readiness on a **0–10 scale**.

| Condition | Points |
|---|---|
| `campaign-spec.md` fully filled (all fields present) | +3 |
| `key_topics` has ≥ 2 entries | +2 |
| ≥ 1 KOL has 2–3 bullets | +2 |
| `campaign_tone` and `campaign_goal` both present | +1 |
| ≥ 5 KOLs provided | +2 |

> **Note:** Clarifying Questions (Section 4) are a **separate gating criterion** — they are not counted in this 0–10 score. Even a perfect score of 10 here does not unlock generation if any clarifying question is unanswered. Both gates must pass independently.

**STOP rules — skill halts and requests fixes before proceeding:**

| STOP Condition | Message |
|---|---|
| Gate score < 6 | `⛔ STOP: Readiness [X]/10. Minimum is 6. Missing: [list]. Please complete before running.` |
| `key_topics` is empty | `⛔ STOP: key_topics is empty. Cannot score Relevance without it.` |
| `offer` missing | `⛔ STOP: offer is missing. Offer is required to write outreach (even if not mentioned in DM1).` |
| `cta` missing | `⛔ STOP: cta is missing. cta is required to write the Soft CTA after DM1 replies.` |
| All KOLs have 0 bullets | `⛔ STOP: No KOL has recent_content_bullets. All DMs will be [GENERIC]. Confirm you want to proceed, or add bullets first.` |
| Any clarifying question unanswered | `⛔ STOP (Clarifying Gate): Q[N] is unanswered. Readiness score does not override this. Please answer before I continue.` |

> If gate score ≥ 6 and no STOP condition is triggered → display score and proceed: `✅ Readiness: [X]/10 — proceeding.`
> **Note:** `offer` and `cta` are required even though DM1 is not a pitch; they define the post-reply next step.

---

## 8. Output Structure

All output is returned as **one Markdown block**, ready to copy. Three sections in order:

---

### Section 1 — KOL Scorecard Summary

> **Sort Order:** The table must be grouped by Tier (A → B → C) and then sorted by Score (descending) within each Tier.

```markdown
## KOL Scorecard — [Campaign Name]
Readiness: [X]/10 | Run date: [date] | Total KOLs: [N] | Tier A: [N] | Tier B: [N] | Tier C: [N]

| Handle | Tier | Score | Relevance | Content Fit | Engagement | Language | Note |
|---|---|---|---|---|---|---|---|
| @handle1 | 🟢 A | 86 | 34/40 | 26/30 | 16/20 | 10/10 | Strong fit on [topic] |
| @handle2 | 🟡 B | 65 | 22/40 | 21/30 | 12/20 | 10/10 | Partial fit — tone mismatch |
| @handle3 | 🔴 C | 43 | 10/40 | 11/30 | 12/20 | 10/10 | Off-topic, skip |
```

---

### Section 2 — KOL Analysis (per KOL)

```markdown
### @handle1
- **Tier:** 🟢 A | **Score:** 82/100
- **Relevance:** [1–2 sentences using bullets as evidence — or "[generic/no evidence]" if missing]
- **Content style:** [observation on tone/format derived from bullets only]
- **Engagement:** [follower bracket applied — e.g., "100k–499k → 16 pts"]
- **Language:** [match / mismatch with target_market_language]
- **Red flags:** [if any — e.g., "all bullets appear off-topic from key_topics"]
- **Recommendation:** ✅ Priority outreach | ⏸ Hold | ❌ Skip
```

---

### Section 3 — DM Drafts (Tier A and B only)

```markdown
### DM — @handle1 (Tier A)

**DM1 — Opinion Ask (no pitch, no collab mention):**
> [Message referencing 1–2 real bullets from KOL's recent content]
> Goal: ask for their perspective on a topic relevant to their work — zero pitch, zero product mention

**Soft CTA (use only if KOL replies to DM1):**
> [One sentence — request 10-min chat or quick take]

**Personalization source:** "[exact bullet text used — verbatim copy-paste, do not paraphrase or alter]"

> **Rule:** Personalization source must be a verbatim copy-paste of the bullet used. Do not paraphrase or alter the source text.

---

**Follow-up DM — Tier A only (send at 24–48h if no reply to DM1):**
> [Re-engagement using a different angle — references second bullet if available,
>  or adjacent topic from niche_tags. Still zero pitch.]

**Follow-up personalization source:** "[bullet used — verbatim copy-paste — or 'niche_tags only — not presented as observed content'"]
```

> **DM guardrails:**
> - DM1 must not mention: collab, partnership, product name, campaign, paid, gifted
> - **DM1 length: ≤ 300 characters** (optimized for mobile screen scanning on X)
> - **Follow-up DM length: ≤ 200 characters** (low-pressure bump — shorter = less imposing)
> - If KOL has no bullets → prepend `⚠️ [GENERIC — review before sending]` to the DM block
> - `niche_tags` may inform topic angle for follow-up DM but must **never** appear as fabricated evidence (never write "I saw your post about X" if X only came from niche_tags)
> - Soft CTA is always a separate block — never pre-merged into DM1
> - Follow-up DM applies to Tier A only

---

## 9. Workflow

```
INPUT
 ├── campaign-spec.md
 └── KOL list (paste)
        │
        ▼
STEP 0 — Clarifying Questions
  Ask Q1 (Direction), Q2 (Tone), Q3 (Hard Constraints)
  Wait for all 3 answers — do not proceed on partial answers
        │
        ▼
STEP 1 — Readiness Gate
  Score 0–10 → apply STOP rules if any triggered
  Display gate score before continuing
        │
        ▼
STEP 2 — Parse & Validate
  Extract key_topics, tone, language, goal from campaign spec
  Per KOL: check all fields present; flag missing bullets
        │
        ▼
STEP 3 — Score Each KOL
  Apply 4-dimension rubric
  Apply bullet cap rule if triggered (Relevance + Content Style Fit ≤ 20 combined)
  Assign Tier A / B / C
        │
        ▼
STEP 4 — Write Analysis
  Relevance + Content Style: reference bullets as evidence
  Write [generic/no evidence] where bullets are missing or < 2
  Engagement: follower_count bracket only — no inferences
        │
        ▼
STEP 5 — Draft DMs
  Draft DM1 as a pure opinion ask (enforce G4: zero collab/product/campaign signals)
  Enforce char limits (DM1 ≤ 300, Follow-up ≤ 200); trim if needed
  Tier A: DM1 + Soft CTA + Follow-up DM (24–48h)
  Tier B: DM1 + Soft CTA only
  Tier C: no DM generated
  Tag [GENERIC] where bullets are missing
  niche_tags: topic angle only, never as fabricated evidence
        │
        ▼
OUTPUT
  └── One Markdown block: Scorecard → Analysis → DM Drafts
```

---

## 10. Guardrails (Non-Negotiable)

| # | Rule |
|---|---|
| G1 | Only use data provided by the user. No web fetching, no crawling, no hallucination. |
| G2 | `recent_content_bullets` missing or < 2 bullets → write `[generic/no evidence]`; cap Relevance + Content Style Fit combined ≤ 20 pts. |
| G3 | DM personalization must reference real bullets only. `niche_tags` may suggest topic angle for follow-up DM but must never be framed as observed content. |
| G4 | DM1 must contain zero collab/product/campaign signals. No exceptions. |
| G4b | DM1 ≤ 300 characters. Follow-up DM ≤ 200 characters. If a draft exceeds the limit, trim by removing generic pleasantries first, then shortening the CTA, while strictly preserving the personalization evidence. |
| G5 | Soft CTA is always a separate block — never merged into DM1. |
| G6 | Campaign spec missing any field → write `MISSING` for that field; halt scoring of dependent dimension. |
| G7 | Engagement score = follower_count bracket only. Do not infer post frequency, engagement rate, or content quality from any other field. |
| G8 | Readiness Gate < 6 or any STOP condition triggered → halt entirely; list what's missing; do not partially generate output. |
| G9 | Hard STOP if offer or cta is missing (required for DM writing). |

---

## 11. Acceptance Tests

**Test 1 — Happy Path**
- Input: fully filled campaign spec + 12 KOLs each with 2–3 bullets, all clarifying questions answered
- Expected: all KOLs scored correctly; DM1 references real bullets; no [GENERIC] tags; Tier assignments match manual rubric calculation ±0 pts
- Pass condition: every Tier A KOL has a Follow-up DM; every Tier B has DM1 + Soft CTA only; Tier C has no DM

**Test 2 — Missing Bullets Guardrail**
- Input: 4 KOLs with 0 bullets, 4 with 1 bullet, 4 with 2–3 bullets
- Expected: 0-bullet and 1-bullet KOLs capped ≤ 20 pts on Relevance + Content Style Fit combined; all tagged [generic/no evidence]; none reach Tier A
- Pass condition: no fabricated evidence appears in any DM for capped KOLs

**Test 3 — STOP Rule Trigger**
- Input: campaign spec with empty `key_topics`; clarifying Q2 skipped
- Expected: skill outputs `⛔ STOP` messages listing both issues; zero scoring output produced
- Pass condition: skill does not generate any scorecard, analysis, or DM until both issues are resolved

**Test 4 — Missing offer/cta STOP Rule Trigger**
- Input: campaign spec missing `offer` or `cta`
- Expected: skill outputs `⛔ STOP` message; zero scoring output produced
- Pass condition: skill outputs `⛔ STOP` message for the missing field(s) via the dedicated offer/cta STOP rule (independent of Gate score); zero scoring or DM output is produced.

**Test 5 — Mixed Language Campaign**
- Input: campaign spec with `target_market_language: mixed`; KOL list includes 1 EN KOL and 1 CN KOL
- Expected: both EN and CN KOLs score 10/10 on Language Match
- Pass condition: Language Match is not assigned arbitrarily; both supported languages receive full score under mixed campaign

---

## 12. Edge Cases

**Edge Case 1 — KOL language doesn't match target_market_language**
- Example: KOL posts in EN, campaign targets CN market
- Handling: Language Match = 0 pts; flagged in analysis as "language mismatch"; recommendation set to ⏸ Hold unless total score still reaches Tier A/B threshold without language pts

**Edge Case 2 — KOL has 2–3 bullets but all are off-topic from key_topics**
- Handling: Relevance scored low (≤ 10 pts); Content Style scored on tone only (no topic connection); red flag noted ("bullets present but unrelated to key_topics"); no fabricated alignment to campaign

**Edge Case 3 — Duplicate handles or identical profile_urls**
- Handling: skill outputs `⚠️ Duplicate detected: @handle appears [N] times` before scoring begins; asks user to confirm which entry to keep; does not score duplicates until resolved

---

## 13. MVP Scope

**IN SCOPE (Day 1–2):**
- ✅ Mandatory clarifying questions (3) with hard gate before any output
- ✅ Readiness Gate 0–10 with STOP rules
- ✅ Parse campaign spec .md + KOL list paste
- ✅ Score 12 KOLs across 4 dimensions with fixed thresholds
- ✅ Assign Tier A / B / C
- ✅ Per-KOL analysis referencing real bullets
- ✅ DM1 + Soft CTA for Tier A and B
- ✅ Follow-up DM (24–48h) for Tier A only
- ✅ One copy-ready Markdown output block

**OUT OF SCOPE (MVP):**
- ❌ Fetching data from X/Twitter or any external source
- ❌ Auto CSV/PDF export
- ❌ Multi-campaign batch runs
- ❌ DM sequence beyond the 24–48h follow-up
- ❌ Blacklist/whitelist management

**Roadmap (post-MVP):**
- **Phase 2 (Auto-pull + Delivery):** Auto-pull KOL data via PhantomBuster or Chrome extension (twitter-web-exporter) → Claude Skill normalizes + scores + drafts DMs → Telegram bot delivers output to BD. OpenClaw orchestrates full pipeline.
- **Phase 2.5 (Audience Quality):** Mine KOL's tweet replies/comments → extract audience sentiment → add "Audience Quality" as new scoring dimension to evaluate whether the KOL's followers are genuinely engaged or low-quality.
- **Phase 3 (Engagement + Outcome Loop):** Per-tweet engagement metrics (avg likes/replies/reposts) as 5th scoring dimension → re-rank KOLs → track DM reply rates → feedback loop to continuously improve scoring weights.
- **Phase 4 (Scale Ops):** Export CSV/Notion-ready blocks + A/B hook variants for DM1. Add "Brand Safety" dimension to rubric. Batch mode for multi-campaign KOL overlap comparison.

---

## 14. Demo Plan (Day 4)

| Step | Content | Duration |
|---|---|---|
| 1 | Problem framing — before/after | 2 min |
| 2 | Show clarifying questions firing + answers | 1 min |
| 3 | Paste real `campaign-spec.md` → Readiness Gate output live | 1 min |
| 4 | Paste 12 real KOLs → full scoring run | 2 min |
| 5 | Walk through Scorecard + 2 Analysis examples | 2 min |
| 6 | Show DM1 vs Follow-up DM for 1 Tier A KOL | 1 min |
| 7 | Demonstrate guardrail: KOL with no bullets → [generic/no evidence] + cap applied | 1 min |
| 8 | Q&A | open |

---

## 15. Success Criteria

Skill is considered successful if:
- [ ] Runs live with 12 real KOLs without errors or hallucinated data
- [ ] Tier assignments are manually verifiable against rubric ±0 pts
- [ ] DMs are personalized from real bullets — no invented evidence
- [ ] [GENERIC] tag fires correctly on KOLs missing bullets
- [ ] STOP rules fire correctly when inputs are incomplete
- [ ] BGK can copy a DM draft and use it with minimal or no editing

---

*spec.md — v1.6.0 | Day 1 | Language: English | Approved*
