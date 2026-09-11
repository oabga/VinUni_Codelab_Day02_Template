# 🏗️ Phase 3 — DEEP-DIVE & Phase 5 — EVALUATE: Báo Cáo Phân Tích Sâu Dự Án AI

> **Dự án Lựa Chọn:** Trợ Lý AI Thẩm Định & Duyệt Hồ Sơ Đăng Ký Thi Công Nội Thất / Cải Tạo Căn Hộ trên App Vinhomes Resident  
> **Đơn vị áp dụng:** Vinhomes (Khối Ban Quản Lý Đô Thị)  
> **Nhóm thực hiện:** AI Product Engineering Team — Vin Smart Future  

---

# 🏗️ Phase 3 — DEEP-DIVE (Báo Cáo Phân Tích Sâu)

## 3.1. Current-State Workflow Mapping (Sơ đồ quy trình hiện tại)

Quy trình xử lý & thẩm định đơn đăng ký thi công nội thất căn hộ của Ban Quản Lý Vinhomes hiện tại:

```text
┌────────────────┐     ┌────────────────┐     ┌────────────────┐     ┌────────────────┐
│ Bước 1         │     │ Bước 2         │     │ Bước 3         │     │ Bước 4         │
│ Cư dân nộp hồ  │     │ Tra cứu & đối  │     │ Kiểm tra vi    │     │ Soạn bản thẩm  │
│ sơ PDF qua App │ ──> │ chiếu bản vẽ   │ ──> │ phạm kết cấu   │ ──> │ định & lập biên│
│ Resident       │     │ gốc của dự án  │     │ & an toàn PCCC │     │ bản ký duyệt   │
│ Ai: Cư dân     │     │ Ai: BQL Staff  │     │ Ai: BQL Staff  │     │ Ai: BQL Staff  │
│ ⏱ 15 phút      │     │ ⏱ 60 phút 🔴   │     │ ⏱ 120 phút 🔴  │     │ ⏱ 45 phút      │
│ Out: File PDF  │     │ Out: CAD/Data  │     │ Out: Checklist │     │ Out: Biên bản  │
└────────────────┘     └────────────────┘     └────────────────┘     └────────────────┘
                                                                              │
                                                                              ▼
                                                                       ┌────────────────┐
                                                                       │ Bước 5         │
                                                                       │ Trưởng BQL ký  │
                                                                       │ & gửi phản hồi │
                                                                       │ Ai: Trưởng BQL │
                                                                       │ ⏱ 24 - 48h 🔴  │
                                                                       └────────────────┘
🔴 = Bottlenecks (Điểm tắc nghẽn tốn thời gian)
⏱ Tổng thời gian xử lý thủ công: 3 - 5 ngày làm việc / hồ sơ.
```

---

## 3.2. Problem Statement (6-Field) — Standards of Vin Smart Future

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Chuyên viên Kỹ thuật & Trưởng Ban Quản Lý Đô Thị Vinhomes. |
| **2. Current Workflow** | Khi cư dân muốn sửa nhà/làm nội thất, họ gửi bản vẽ PDF & Đơn đăng ký trên App Vinhomes Resident. Chuyên viên BQL mở thủ công bản vẽ PDF, mở file CAD kiến trúc gốc của tòa nhà để đối chiếu từng bức tường, kiểm tra phương án đục phá, đường điện âm tường, hệ thống đầu báo PCCC, rồi viết tay biên bản thẩm định trình Trưởng BQL ký duyệt. |
| **3. Bottleneck** | **Bước 2 & 3 (Kiểm tra kết cấu & PCCC):** Mất 2-3 tiếng/hồ sơ để rà soát thủ công các thông số bản vẽ, so khớp với danh mục quy định cấm của tòa nhà. Khi cao điểm bàn giao căn hộ, hàng trăm hồ sơ dồn ứ khiến BQL bị quá tải nghiêm trọng. |
| **4. Business Impact** | - Thời gian cấp phép kéo dài **3-5 ngày làm việc**, gây bức xúc cho cư dân và chậm tiến độ thi công.<br>- Rò rỉ rủi ro an toàn: Nếu chuyên viên nhìn sót hành vi đục tường chịu lực hoặc che lấp đầu báo PCCC, rủi ro tai nạn kết cấu và PCCC cho toàn bộ tòa nhà là cực kỳ lớn. |
| **5. Success Metric** | 1. **Hiệu suất (Efficiency):** Giảm tổng thời gian thẩm định hồ sơ từ 3 ngày xuống **dưới 4 giờ làm việc** (rút ngắn 90%).<br>2. **Chính xác (Quality):** Tỉ lệ trích xuất và phát hiện vi phạm quy hoạch kết cấu/PCCC đạt **>= 95%**. |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** Đọc file PDF/Bản vẽ, trích xuất dữ liệu mặt bằng, kiểm tra luật quy hoạch tòa nhà, soạn bản nháp thẩm định dạng `[DRAFT_ONLY]` kèm lý do chi tiết.<br>🛑 **CẤM (STRICT BOUNDARY):** AI tuyệt đối KHÔNG được tự động phát hành giấy phép thi công gửi cho cư dân khi chưa có chữ ký duyệt điện tử của Trưởng Ban Quản Lý (HITL); KHÔNG được bỏ qua bất kỳ cảnh báo vi phạm tường chịu lực nào. |

---

## 3.3. Future-State Flow & AI Fit (Quy trình tương lai có AI)

* **Phân loại AI Fit:** **LLM Feature + Document Intelligence (OCR & Vision Model)**.
* **Mô tả quy trình tương lai:**

```text
┌────────────────┐     ┌────────────────┐     ┌────────────────┐     ┌────────────────┐
│ Bước 1         │     │ Bước 2         │     │ Bước 3         │     │ Bước 4         │
│ Cư dân nộp file│     │ 🔵 AI Vision   │     │ 🔵 AI Audit    │     │ 🟢 Trưởng BQL  │
│ PDF bản vẽ trên│ ──> │ OCR & trích    │ ──> │ Rules & draft  │ ──> │ click phê duyệt│
│ App Resident   │     │ xuất mặt bằng  │     │ [DRAFT_ONLY]   │     │ trên Dashboard │
└────────────────┘     └────────────────┘     └────────────────┘     └────────────────┘
                                                                              │
                                                                              ▼
                                                                       ↩️ Fallback:
                                                                       Nếu bản vẽ mờ/lỗi
                                                                       format, AI đẩy về
                                                                       BQL soi tay lại.
```

### Chi tiết các bước AI & Ranh giới an toàn:
* 🔵 **AI Step (Automation):** Trích xuất tự động vị trí căn hộ, kiểm tra sơ đồ điện/nước/tường cải tạo với sơ đồ gốc của Vinhomes. Tự động gắn nhãn vi phạm nếu phát hiện: *(1) Đục tường chịu lực, (2) Thay đổi vị trí hộp kỹ thuật, (3) Lắp trần thạch cao che lấp đầu báo PCCC.*
* 🟢 **Human Step (HITL):** Trưởng BQL xem lại kết quả phân tích nháp `[DRAFT_ONLY]`, bấm "Chấp thuận" hoặc "Yêu cầu chỉnh sửa".
* ↩️ **Fallback Plan:** Nếu file bản vẽ của cư dân bị mờ, thiếu tỉ lệ, hoặc format không chuẩn, hệ thống tự động trả về cảnh báo `{"status": "NEED_MANUAL_REVIEW", "reason": "Drawing scale unreadable"}` để chuyên viên BQL kiểm tra thủ công như quy trình cũ.

---

# 🏁 Phase 5 — EVALUATE (Đánh Giá Độ Sẵn Sàng & Ra Quyết Định)

### AI Readiness Checklist:
1. [x] **Dữ liệu sạch/Mẫu logs:** Vinhomes sở hữu 100% bản vẽ kiến trúc CAD gốc của tất cả các tòa nhà và hàng nghìn hồ sơ thi công đã duyệt trước đây để huấn luyện/eval.
2. [x] **Tầm kiểm soát rủi ro:** Rủi ro được kiểm soát 100% nhờ cơ chế Human-in-the-loop (bắt buộc chữ ký số của Trưởng BQL) và tag nháp `[DRAFT_ONLY]`.
3. [x] **Sự sẵn sàng của Stakeholders:** Ban Quản Lý Đô Thị Vinhomes rất mong muốn giảm tải áp lực hành chính và nâng cao chỉ số hài lòng (NPS) của cư dân.

---

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Phát triển bản thử nghiệm (Pilot) tại 2 đô thị Vinhomes Ocean Park và Vinhomes Grand Park.

**Justification (Lý giải quyết định):**
> *Bài toán có thông số metric rõ ràng, giải pháp công nghệ khả thi cao (kết hợp OCR Vision + LLM Feature), dữ liệu đầu vào chuẩn hóa tốt, và mang lại giá trị kinh tế trực tiếp (tiết kiệm hàng nghìn giờ làm việc cho BQL và nâng tầm trải nghiệm cư dân Vinhomes).*
