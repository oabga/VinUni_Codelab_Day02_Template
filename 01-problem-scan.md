# 🔍 Phase 1 — SCAN & Phase 2 — QUICK-ASSESS (Ý tưởng Cá nhân)

---

## 🏛️ Bối cảnh: Vin Smart Future (Vingroup)
**Họ và tên học viên:** Lê Gia Bảo
**Mảng ưu tiên:** Vinhomes (Ban Quản Lý Đô Thị) / Vingroup Ecosystem  

---

# 🔍 Phase 1 — SCAN (Cá nhân)

Dưới đây là danh sách **5 bài toán/bottleneck thực tế** được quét qua các công ty thành viên thuộc Tập đoàn Vingroup sử dụng **4 Lenses**:

| # | Subsidiary | Lens | Mô tả ngắn bài toán / Bottleneck thực tế |
|---|------------|------|------------------------------------------|
| 1 | **Vinhomes** | Tốn thời gian | **Thẩm định hồ sơ thi công nội thất căn hộ:** Ban quản lý mất 3-5 ngày đọc thủ công bản vẽ PDF, kiểm tra diện tích, kế hoạch đục tường chịu lực, phương án PCCC trước khi cấp phép. |
| 2 | **VinFast** | AI-upgrade | **Chẩn đoán lỗi xe điện từ mô tả tiếng Việt:** Khách hàng mô tả triệu chứng xe (ví dụ: *"đi qua gờ giảm tốc kêu cụp cụp ở bánh trước"*), hệ thống tự động phân loại và gợi ý mã DTC (Diagnostic Trouble Code) cho kỹ thuật viên xưởng dịch vụ. |
| 3 | **Xanh SM** | Pain từ người khác | **Phân loại vi phạm quy chuẩn dịch vụ:** Tự động phân tích ghi âm cuộc gọi CSKH và đánh giá 1-star của khách hàng về tài xế (thái độ, mùi xe, lái ẩu) để phân loại lỗi và đề xuất đào tạo lại tài xế. |
| 4 | **Vinpearl** | Lặp lại | **Trích xuất yêu cầu & báo giá Booking đoàn:** Đọc email đặt phòng theo đoàn (Group Booking) phức tạp từ đại lý du lịch (yêu cầu phòng nối liền, ăn kiêng, xe đưa đón) để đối chiếu quỹ phòng và draft báo giá. |
| 5 | **Vinmec** | Tốn thời gian | **Tóm tắt hồ sơ xuất viện cho bệnh nhân:** Trích xuất kết quả xét nghiệm/lâm sàng phức tạp để tự động soạn tóm tắt xuất viện bằng ngôn ngữ bình dân, dễ hiểu cho bệnh nhân và người nhà. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

---

### 📋 QUICK PROBLEM CARD #1 (Lựa chọn hàng đầu cho Deep-Dive)

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                       │
│                                                                             │
│ Bài toán: Thẩm định & Cấp phép Hồ sơ Đăng ký Thi công Nội thất / Cải tạo   │
│ căn hộ cư dân trên App Vinhomes Resident.                                   │
│ Công ty thành viên: [x] Vinhomes                                            │
│                                                                             │
│ Ai đang đau (Actor)? Ban Quản Lý Đô Thị (quá tải), Cư dân (chờ lâu)         │
│                                                                             │
│ Workflow thủ công hiện tại (5 bước):                                        │
│   1. Cư dân nộp file PDF bản vẽ & đơn đăng ký trên App                     │
│   ──> 2. Chuyên viên BQL đọc bản vẽ, đối chiếu bản vẽ gốc dự án             │
│   ──> 3. Kiểm tra các mục cấm (tường chịu lực, hộp kỹ thuật, PCCC)          │
│   ──> 4. Soạn bản nhận xét thẩm định & lập biên bản ký duyệt thủ công       │
│   ──> 5. Gửi thông báo phê duyệt hoặc yêu cầu sửa đổi cho cư dân           │
│   ⏱ Tổng thời gian: 3 - 5 ngày làm việc / hồ sơ.                            │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 120-180 phút/hồ sơ)           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 & 4                         │
│ (OCR bản vẽ PDF -> Trích xuất thay đổi -> Kiểm tra quy hoạch -> Draft kết quả)│
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│ - Giảm thời gian thẩm định từ 3 ngày ──> dưới 4 giờ làm việc.               │
│ - Tỉ lệ phát hiện lỗi vi phạm kết cấu/PCCC chính xác đạt >= 95%.            │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (Tích hợp OCR + Rules Engine)            │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📋 QUICK PROBLEM CARD #2

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                       │
│                                                                             │
│ Bài toán: Chẩn đoán sơ bộ mã lỗi kỹ thuật xe điện (DTC) từ mô tả Tiếng Việt│
│ của khách hàng tại Xưởng Dịch Vụ VinFast.                                   │
│ Công ty thành viên: [x] VinFast                                             │
│                                                                             │
│ Ai đang đau (Actor)? Cố vấn dịch vụ (SA) và Kỹ thuật viên Xưởng VinFast     │
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Khách mang xe đến hoặc gọi hotline tả triệu chứng bằng tiếng Việt      │
│   ──> 2. Cố vấn dịch vụ ghi chép tay lại triệu chứng vào hệ thống CRM       │
│   ──> 3. Kỹ thuật viên cắm máy chẩn đoán OBVi đọc danh sách 20+ mã DTC      │
│   ──> 4. Tra cứu sổ tay kỹ thuật để khoanh vùng linh kiện nghi hỏng           │
│   ⏱ Tổng thời gian: 45 - 60 phút / lượt tiếp nhận.                          │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 4 (⏱ 30 phút/lượt)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 4                             │
│ (LLM phân tích ngôn ngữ tự nhiên -> Map với triệu chứng cơ học -> Draft gợi ý)│
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│ - Rút ngắn thời gian tiếp nhận & khoanh vùng lỗi từ 45 min ──> under 10 min. │
│ - Tỉ lệ gợi ý đúng nhóm linh kiện đạt 90%.                                  │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (Map tiếng Việt tự do -> DTC Code)       │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📋 QUICK PROBLEM CARD #3

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                       │
│                                                                             │
│ Bài toán: Tự động phân loại vi phạm quy chuẩn dịch vụ và cảnh báo rủi ro     │
│ từ phản hồi khách hàng Xanh SM.                                             │
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau (Actor)? Team Kiểm Soát Chất Lượng (QA/QC) Xanh SM              │
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Hệ thống thu thập các chuyến xe chấm 1-3 sao & cuộc gọi phàn nàn        │
│   ──> 2. QA nghe lại file âm thanh / đọc comment của khách                  │
│   ──> 3. Ghi chép mã lỗi vi phạm (thái độ, thu quá tiền, lái ẩu, mùi xe)     │
│   ──> 4. Lập báo cáo đề xuất chế tài phạt hoặc triệu tập đào tạo lại        │
│   ⏱ Tổng thời gian: 20 phút / case review.                                  │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 15 phút/case)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3                             │
│ (Speech-to-Text + LLM Sentiment Audit -> Phân loại lỗi vi phạm tự động)     │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│ - Tăng số lượng ticket được audit từ 10% ──> 100% tổng số phản hồi xấu.     │
│ - Giảm thời gian ra quyết định xử lý vi phạm từ 24h ──> under 1h.           │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (Sentiment & Compliance Auditor)         │
└─────────────────────────────────────────────────────────────────────────────┘
```
