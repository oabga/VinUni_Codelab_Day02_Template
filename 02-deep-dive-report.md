# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên Chăm sóc khách hàng (CSKH) / Điều phối viên Xanh SM Express. |
| **2. Current Workflow** | Đọc ticket thủ công ──> Tra cứu hệ thống TMS ──> Gõ phản hồi xử lý sự cố. |
| **3. Bottleneck** | Đọc hiểu, phân loại cảm xúc/intent của khách hàng (xử lý ngôn ngữ tự động) và tốn thời gian soạn nháp phản hồi cá nhân hóa. |
| **4. Business Impact** | Tồn đọng ticket trong giờ cao điểm làm giảm độ tin cậy của dịch vụ giao hàng chặng cuối. Khách hàng phải chờ SLA phản hồi > 2 tiếng, ảnh hưởng nghiêm trọng đến customer experience. |
| **5. Success Metric** | Tự động phân loại 95% ticket dưới 5s. Sinh dự thảo phản hồi giảm thời gian xử lý trung bình (AHT) từ 12 phút ──> dưới 3 phút/lượt. |
| **6. Operational Boundary** | AI ĐƯỢC đọc, phân loại, gọi API tra cứu trạng thái đơn và soạn nháp. TUYỆT ĐỐI KHÔNG ĐƯỢC tự động cấp mã hoàn tiền/voucher đền bù. CẦN DUYỆT nội dung phản hồi trước khi gửi (Human-in-the-loop). |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [x] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** LLM phân tích intent, trích xuất mã vận đơn và mức độ khẩn cấp.
  * 🔵 **AI Step:** Agent tự động gọi API hệ thống TMS để lấy vị trí/trạng thái thực tế của gói hàng.
  * 🔵 **AI Step:** Soạn sẵn dự thảo (Draft) câu trả lời dựa trên knowledge base của công ty.
  * 🟢 **Human Step (HITL):** Nhân viên CSKH đọc lại bản nháp, duyệt (Approve) hoặc chỉnh sửa nhẹ, tick chọn đền bù (nếu có).
  * ↩️ **Fallback:** Nếu hệ thống TMS sập hoặc LLM không nhận diện được intent (Confidence Score < 0.7), tự động bỏ qua bước sinh nháp và đẩy nguyên bản ticket về hàng đợi ưu tiên cho CSKH đọc thủ công.

---
# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*Việc tối ưu khâu giao nhận chặng cuối (Last-mile delivery) đang là yếu tố cốt lõi quyết định lợi thế cạnh tranh của Xanh SM Express. Bài toán xử lý ngôn ngữ và tra cứu đa hệ thống này quá phức tạp để dùng Rule-based, nhưng lại là điểm mạnh tuyệt đối của các mô hình LLM kết hợp Agentic logic. Bằng việc áp dụng Human-in-the-loop, chúng ta triệt tiêu được rủi ro AI "ảo giác" (hallucination) đối với khách hàng, đồng thời giải phóng ngay lập tức ~75% thời gian thao tác vô giá trị của đội ngũ vận hành. Chi phí API cho một lượt xử lý (vài trăm đồng) hoàn toàn không đáng kể so với chi phí lương cho thời gian AHT tiết kiệm được.
---