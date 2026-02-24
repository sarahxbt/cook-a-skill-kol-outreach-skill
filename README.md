# KOL Qualifier & Outreach Drafter

AI Skill cho BD/Marketing: paste campaign spec + danh sách KOL → AI chấm điểm 4 chiều → phân Tier A/B/C → draft DM cá nhân hóa.

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
| `SKILL.md` | System Prompt v1.1 — paste vào Claude Project Instructions |
| `spec.md` | Spec v1.6.0 — đã qua 2 vòng independent review |
| `mock_campaign.md` | Campaign demo: Whales Prediction Waitlist Launch |
| `mock_kols.md` | 12 KOL test data với edge cases |
| `skill-card.md` | Skill Card (1 trang tóm tắt) |

## AI Showcase

Thư mục `ai-showcase/` chứa 8 screenshot chứng minh quá trình làm việc với AI, theo thứ tự timeline:

1. `01-ideation-brainstorm-claude.png` — 🧠 Brainstorm ý tưởng skill cùng Claude (Sonnet 4.6)
2. `02-self-critique-prompt-weaknesses.png` — 🔍 Claude tự phản biện, chỉ ra 6 điểm yếu trong prompt
3. `03-multi-ai-review-pipeline.png` — 🔄 Viết review prompt cho AI khác đọc cold-read (multi-AI pipeline)
4. `04-human-in-loop-selective-apply.png` — 👤 Human-in-the-loop: apply 7 findings, loại bỏ CAT-2-02
5. `05-spec-finalized-build-skill.png` — 🏗️ Spec hoàn chỉnh (bỏ VN language), bắt đầu viết SKILL.md
6. `06-compliance-audit-checklist.png` — 📋 AI audit toàn bộ GUIDEBOOK requirements (PASS/FAIL/PARTIAL)
7. `07-ai-automated-dry-run.png` — 🤖 AI tự chạy dry-run (không cần bật Claude thủ công)
8. `08-final-all-guardrails-pass.png` — ✅ Kết quả dry-run: tất cả guardrails PASS, score khớp 100%


## How to Run

1. Tạo Claude Project mới
2. Paste toàn bộ `SKILL.md` vào **Project Instructions**
3. Trong chat, paste `mock_campaign.md` + `mock_kols.md`
4. Trả lời 3 Clarifying Questions → Skill tự chạy

## Edge Cases Demo

- **Duplicate KOL** → Skill halt, hỏi user xác nhận
- **0 bullets** → Guardrail cap Relevance + Content ≤ 20, tag `[GENERIC]`
- **Language mismatch** → Language Match = 0
- **Off-topic content** → Relevance rơi vào band None/Tangential
- **Follower = 0 hoặc rất thấp** → Bracket < 10k = 4 pts (valid, NOT invalid)

---

*Cook A Skill — Day 4 | By Sarah*