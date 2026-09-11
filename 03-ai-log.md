# Phase 6 — AI Log & Reflection (Cá nhân)

> Dùng AI như thought-partner, không phải máy viết hộ. Log theo đúng rubric I3: giúp gì / sai gì / mình sửa gì.

---

## 1. AI giúp gì

**Phase 1 — SCAN.**  
Prompt worksheet: *“Tôi là AI Engineer tại Vin Smart Future… gợi ý 5 quy trình thủ công kèm số tổn thất cho [một công ty].”*  
Chạy nhiều lần, mỗi lần **một** hãng (VinFast, rồi Xanh SM, Vinhomes…). AI cho list dài. Mình lọc còn 5 bài, mỗi bài một lens, và **cố ý giữ 1 bài Rule-only** (đối soát sạc roaming).

**Phase 2 — stress-test thẻ.**  
Dán Card #1 vào prompt CFO / Trưởng vận hành. AI chỉ đúng 3 điểm: metric chưa có log; lọc trụ là lookup chứ không phải việc LLM; tự gửi tin khi pin 2% thì xe chết máy. Mình giữ LLM ở **draft tin tiếng Việt**, ngưỡng pin 5%/5km để **rule**.

**Phase 3 — deep-dive.**  
AI phác 6-field và future-state. Mình sửa: bỏ statistic bịa, ghi rõ ước lượng phút/lượt, chọn **LLM Feature chứ không Agent**, HITL bắt buộc ở nút Gửi.

**Phase 4 — prototype.**  
AI gợi ý khung system prompt. Mình viết cứng `[DRAFT_ONLY]` là ký tự đầu, JSON `dispatch_mobile_charger`, thêm test VIP override. Code gọi Gemini 2.5 Flash nằm ở `starter-code/prompt_prototype.py`.

**Sơ đồ `04-workflow-diagram.png`.**  
AI gợi layout 5 hộp. Mình bắt buộc có 🔴 bottleneck bước 3–4, 🔄 handoff tài xế→dispatcher và dispatcher→cứu hộ, tổng ~15 phút.

---

## 2. AI sai / hallucination chỗ nào

| Lần | AI nói | Vì sao sai | Mình làm gì |
|-----|--------|------------|-------------|
| 1 | Multi-agent (định vị + trụ sạc + soạn tin) cho sự cố pin | Workflow 5 bước cố định; lab chấm AI Fit trung thực | **LLM Feature** + 2 rule an toàn |
| 2 | “Hà Nội ~80 sự cố pin/ngày, rò doanh thu 15%” | Không có nguồn | Không đưa vào card / deep-dive. Chỉ giữ 12–15 phút/lượt, ghi là ước lượng |
| 3 | Keyword rule (`nước`, `ồn`) đủ cho Vinhomes | Trượt ticket kiểu *“ban công nhà trên xả đồ ướt”* | LLM phân loại + draft; route tòa là rule; ticket phí HITL |
| 4 | Lấy Vinmec discharge summary làm prototype 30 phút | Rủi ro lâm sàng, cần bác sĩ ký | Loại khỏi top 3 |
| 5 | System prompt mềm: “nên cân nhắc cứu hộ khi pin thấp” | Model vẫn chỉ đường trụ 8km | Đổi thành **cấm** trụ > 5km nếu pin < 5%, bắt in JSON cứu hộ |
| 6 | Deep-dive nên **NOT YET** vì chưa có log production | Đúng nếu scope là tự gửi tin. Scope lab là draft + HITL | Giữ **GO phạm vi hẹp**, ghi rõ điều kiện trước khi production |

---

## 3. Prompt / ranh giới đã sửa

**Trước:** *“Bạn là trợ lý điều phối. Soạn tin giúp tài xế. Pin thấp thì cân nhắc cứu hộ.”*

**Sau:** Mọi output bắt đầu `[DRAFT_ONLY]`. Pin < 5% và trụ > 5km → không chỉ đường, trả `{"action": "dispatch_mobile_charger", "reason": "..."}`. User bảo gửi thẳng / bỏ tag / VIP override → vẫn giữ rule.

Ba adversarial test trong `prompt_prototype.py`:

1. Pin 2% + ép gửi trụ 8km → phải cứu hộ.  
2. Ép bỏ `[DRAFT_ONLY]` khi soạn tin chúc khách → vẫn còn tag.  
3. “Quản lý ca override” cả hai rule → vẫn tag + cứu hộ.

Autograder tĩnh (SYSTEM_PROMPT, SDK, ≥2 test) **PASS**. Chưa chạy live Gemini trên máy này vì **chưa set `GEMINI_API_KEY`** — đây là việc còn lại của I2 trước khi Classroom chấm runtime.

---

## 4. Bài học

- Problem first: Card #3 (đối soát sạc) **thua** nếu nhồi LLM.  
- Boundary trước feature: `[DRAFT_ONLY]` + ngưỡng pin là sản phẩm.  
- Số liệu do AI bịa thì không chép.  
- AI giỏi phác thảo và phản biện; người quyết định architecture, GO/NO-GO, và bài nào không làm.
