# Skill Card — KOL Qualifier & Outreach Drafter

**Owner:** Vũ Phương Anh (Sarah)
**Cook A Skill — Day 4 | Platform: Claude Project**

| Item | Details |
|:---|:---|
| **Skill Name** | KOL Qualifier & Outreach Drafter |
| **What is automated** | Evaluates and tiers a KOL list against a campaign spec, then drafts personalized DMs for each qualified KOL — instead of BD manually reviewing each profile, scoring by gut feel, and writing outreach from scratch. |
| **BEFORE: manual process** | BD receives a product spec → scrolls through each KOL's Twitter → evaluates fit by gut feel (good match or not?) → writes a generic copy-paste DM for all. **Takes hours for 10–15 KOLs**, low outreach quality, inconsistent personalization. |
| **AFTER: with skill** | Paste campaign spec + KOL list into Claude → AI scores across 4 dimensions (Relevance 40 + Content Fit 30 + Engagement 20 + Language 10) → assigns Tier A/B/C → drafts personalized DMs based on the KOL's actual content. **Takes ~5 minutes**, each DM has clear personalization evidence. |
| **Tools/AI used** | Claude (Project mode) — SKILL.md as System Prompt |
| **Limitations** | 1. Cannot auto-crawl KOL data — user must provide the list + content bullets manually. 2. Scoring is based on user-provided bullets, not full content history. 3. DMs are drafts only — BD must review before sending. |
| **Expansion Roadmap** | **Phase 1 (current):** Manual input → Claude Skill scores + drafts DMs → BD reviews and sends. **Phase 2:** Auto-pull KOL data via PhantomBuster or Chrome extension (twitter-web-exporter) → Claude Skill normalizes + scores + drafts DMs → Telegram bot delivers output to BD. OpenClaw orchestrates the full pipeline. **Phase 2.5:** Mine KOL's tweet replies/comments → extract audience sentiment → add "Audience Quality" as new scoring dimension. **Phase 3:** Per-tweet engagement metrics (avg likes/replies/reposts) as 5th scoring dimension → re-rank KOLs → track DM reply rates → feedback loop to improve scoring weights. **Future:** Add "Brand Safety" dimension to rubric; batch mode for multi-campaign KOL overlap comparison. |
