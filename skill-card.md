# Skill Card — KOL Qualifier & Outreach Drafter

| Mục | Nội dung |
|:---|:---|
| **Tên Skill** | KOL Qualifier & Outreach Drafter |
| **Việc gì được automate** | Đánh giá + phân tier danh sách KOL theo campaign spec, rồi draft DM cá nhân hóa cho từng KOL qualified — thay vì BD phải đọc từng profile, chấm điểm cảm tính, rồi viết outreach từ đầu. |
| **TRƯỚC: làm tay** | BD nhận spec sản phẩm → lướt Twitter từng KOL → đánh giá bằng cảm tính (có fit không?) → viết DM generic copy-paste cho tất cả. **Mất 3–4 giờ cho 10–15 KOLs**, outreach quality thấp, reply rate ~5–10%. |
| **SAU: có skill** | Paste campaign spec + KOL list vào Claude → AI chấm điểm 4 chiều (Relevance 40 + Content Fit 30 + Engagement 20 + Language 10) → phân Tier A/B/C → draft DM cá nhân hóa dựa trên content thật của KOL. **Mất ~5 phút**, mỗi DM có personalization evidence rõ ràng. |
| **Tool/AI đã dùng** | Claude (Project mode) — SKILL.md as System Prompt |
| **Limitation** | 1. Không tự crawl KOL data — user phải cung cấp danh sách + content bullets thủ công. 2. Scoring dựa trên bullets user cung cấp, không phải full content history. 3. DM chỉ là draft — BD vẫn cần review trước khi gửi. |
| **Roadmap mở rộng** | 1. Kết nối Twitter API để auto-pull KOL data + recent tweets. 2. Thêm dimension "Brand Safety" vào scoring rubric. 3. A/B test DM variants → track reply rate → feedback loop cải thiện prompt. 4. Batch mode: chạy nhiều campaign cùng lúc, so sánh KOL overlap. |
