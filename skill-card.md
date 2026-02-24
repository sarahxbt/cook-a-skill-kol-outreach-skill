# Skill Card — KOL Qualifier & Outreach Drafter

**Owner:** Vũ Phương Anh (Sarah)
**Cook A Skill — Day 3 | Platform: Claude Project**

| Item | Details |
|:---|:---|
| **Skill Name** | KOL Qualifier & Outreach Drafter |
| **What is automated** | Evaluates and tiers a KOL list against a campaign spec, then drafts personalized DMs for each qualified KOL — instead of BD manually reviewing each profile, scoring by gut feel, and writing outreach from scratch. |
| **BEFORE: manual process** | BD receives a product spec → scrolls through each KOL's Twitter → evaluates fit by gut feel (good match or not?) → writes a generic copy-paste DM for all. **Takes 3–4 hours for 10–15 KOLs**, low outreach quality, reply rate ~5–10%. |
| **AFTER: with skill** | Paste campaign spec + KOL list into Claude → AI scores across 4 dimensions (Relevance 40 + Content Fit 30 + Engagement 20 + Language 10) → assigns Tier A/B/C → drafts personalized DMs based on the KOL's actual content. **Takes ~5 minutes**, each DM has clear personalization evidence. |
| **Tools/AI used** | Claude (Project mode) — SKILL.md as System Prompt |
| **Limitations** | 1. Cannot auto-crawl KOL data — user must provide the list + content bullets manually. 2. Scoring is based on user-provided bullets, not full content history. 3. DMs are drafts only — BD must review before sending. |
| **Expansion Roadmap** | 1. Connect Twitter API to auto-pull KOL data + recent tweets. 2. Add a "Brand Safety" dimension to the scoring rubric. 3. A/B test DM variants → track reply rate → feedback loop to improve the prompt. 4. Batch mode: run multiple campaigns at once, compare KOL overlap. |
