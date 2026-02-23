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

Thư mục `ai-showcase/` chứa các screenshot chứng minh quá trình làm việc với AI:
1. `01-brainstorm-spec-with-claude.png`: Lên ý tưởng spec với Claude
2. `02-claude-analyzing-prompt-weaknesses.png`: Claude phân tích điểm yếu trong adversarial prompt
3. `03-gemini-applying-findings-to-skill.png`: Dùng Gemini để apply các findings vào file SKILL.md


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

*Cook A Skill — Day 3 | By Sarah*