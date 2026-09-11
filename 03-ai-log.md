# 📄 File 03 — AI Log & Reflection (Nhật Ký Tương Tác AI)

> **Học viên:** Nguyễn Văn A  
> **Dự án:** AI Product Scoping (Vin Smart Future — Vinhomes & Ecosystem)  

---

## 🤖 1. AI Làm Trợ Lý Đồng Hành (Thought Partner)

Trong suốt quá trình thực hiện Bài Lab 02, tôi đã sử dụng các công cụ AI (Gemini 2.5 Flash, ChatGPT) làm trợ lý để kích hoạt tư duy và tăng tốc độ làm việc.

### Các công việc AI đã hỗ trợ hiệu quả:
1. **Brainstorm bài toán thực tế (Phase 1):** AI giúp tôi quét qua 4 Lenses vận hành của các công ty thành viên Vingroup (VinFast, Xanh SM, Vinhomes, Vinmec, Vinpearl) và gợi ý các điểm nghẽn thực tế kèm theo con số ước tính rò rỉ hiệu suất.
2. **Phản biện thẻ bài toán (Phase 2):** Tôi đã đóng vai CFO và Trưởng phòng Vận hành khắt khe để prompt AI phản biện Quick Problem Cards, chỉ ra 3 điểm yếu về metric và giúp tôi thắt chặt ranh giới an toàn.
3. **Lập trình bản mẫu System Prompt (Phase 4):** AI hỗ trợ viết cấu trúc System Prompt nghiêm ngặt cho `starter-code/prompt_prototype.py`, xử lý các trường hợp góc (edge cases) khi người dùng cố tình dụ AI phá vỡ ranh giới.

---

## ⚠️ 2. Bài Học Từ Các Lỗi Trả Về Của AI (Hallucination & Limit Cases)

Mặc dù AI phản hồi rất nhanh, tôi đã phát hiện một số lần AI đưa ra câu trả lời sai hoặc chưa chuẩn xác với thực tế vận hành của Vingroup:

1. **AI từng quá tự tin đề xuất tự động duyệt hồ sơ:** Trong lần brainstorm đầu tiên cho bài toán Vinhomes, AI đề xuất cho phép LLM tự động phê duyệt và gửi giấy phép thi công trực tiếp cho cư dân mà không cần con người duyệt.
   * **Phát hiện lỗi:** Tôi nhận ra điều này vi phạm nghiêm trọng quy chuẩn an toàn tòa nhà (rủi ro đục tường chịu lực / PCCC).
   * **Cách khắc phục:** Tôi đã siết chặt Operational Boundary, bổ sung cơ chế bắt buộc **Human-in-the-loop (HITL)** yêu cầu Trưởng Ban Quản Lý ký duyệt điện tử và bổ sung tiền tố `[DRAFT_ONLY]` cho mọi output của AI.

2. **Cố tình bỏ qua tag `[DRAFT_ONLY]` khi bị tấn công prompt (Adversarial Attack):** Trong bài test prompt prototype, khi giả định người dùng nhập câu lệnh ép AI gửi tin đi ngay không cần nháp, mô hình ban đầu bị "xuôi theo" người dùng.
   * **Cách khắc phục:** Tôi đã cập nhật System Prompt với chỉ thị cấp hệ thống (System-level Instruction) có trọng số ưu tiên cao hơn user input, khẳng định thẻ `[DRAFT_ONLY]` là quy tắc bắt buộc không thể thương lượng.

---

## 💡 3. Kết Luận & Bài Học Cá Nhân

- **Product Management First, AI Second:** AI chỉ là công cụ gia tăng hiệu suất. Năng lực cốt lõi của kỹ sư AI tại Vin Smart Future nằm ở việc thấu hiểu quy trình vận hành thực tế (Current-state Workflow), xác định đúng bottleneck và thiết lập ranh giới an toàn (Operational Boundaries) vững chắc.
- **Luôn kiểm chứng output:** Không bao giờ tin tưởng 100% kết quả từ LLM khi chưa qua các bước kiểm thử tự động (Adversarial Tests) và rà soát của con người.
