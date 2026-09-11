# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | VinFast | Predictive Maintenance (IoT) | **Chẩn đoán tình trạng pin (BMS):** Việc xác định các điểm bất thường (outliers) trong dữ liệu cảm biến để bảo hành pin hiện tại thường phải trích xuất thủ công và tính toán bằng các phương pháp thống kê cơ bản (như sử dụng khoảng tứ phân vị - IQR). Quá trình này chậm, thiếu khả năng phân tích chuỗi thời gian đa biến.<br>**Tổn thất ước tính:** Lãng phí **20-30%** thời gian của kỹ thuật viên; chi phí thay thế pin không tối ưu do chẩn đoán sai lệch hoặc rớt nhịp bảo trì lên tới hàng chục tỷ đồng/năm. |
| 2 | VinFast | Computer Vision (QA/QC) | **Kiểm định chất lượng ngoại thất & linh kiện:** Nhân viên KCS phải dùng mắt thường để dò tìm các vết xước sơn hoặc lỗi vi mạch nhỏ giọt trên băng chuyền dưới điều kiện ánh sáng nhà máy, rất dễ bị bỏ sót do tình trạng mỏi mắt ở cuối ca trực.<br>**Tổn thất ước tính:** Lọt lưới lỗi sản phẩm ra thị trường làm tăng tỷ lệ triệu hồi (recall) hoặc bảo hành thêm **1-2%**, gây rò rỉ hàng triệu USD chi phí khắc phục và ảnh hưởng uy tín thương hiệu. |
| 3 | VinFast | Supply Chain Analytics | **Quản lý tồn kho & Dự báo phụ tùng:** Việc đặt hàng phụ tùng thay thế tại các xưởng dịch vụ chủ yếu dựa trên số liệu lịch sử thô, thiếu khả năng dự báo động (dynamic forecasting) theo xu hướng hỏng hóc của từng dòng xe và điều kiện địa lý.<br>**Tổn thất ước tính:** Tỷ lệ tồn kho "chết" (những phụ tùng ít dùng) chiếm tới **15-20%** không gian kho, trong khi thời gian khách hàng phải chờ nhập các linh kiện đặc thù kéo dài thêm **3-5 ngày**, làm giảm trực tiếp chỉ số CSAT. |
| 4 | Vinmec | Computer Vision (Medical Imaging) | **Sàng lọc và phân loại ảnh X-quang/MRI/CT:** Bác sĩ phải đọc thủ công hàng ngàn ảnh y tế mỗi ngày, trong đó ảnh bình thường chiếm đa số. Khung quy trình này thiếu hệ thống AI "flag" các ca bất thường khẩn cấp (như đột quỵ, khối u) để ưu tiên đọc trước.<br>**Tổn thất ước tính:** Lãng phí **30-40%** quỹ thời gian của bác sĩ chuyên khoa sâu; làm chậm trễ các ca cấp cứu "thời gian vàng" và tiềm ẩn **3-5%** nguy cơ sai sót do quá tải. |
| 5 | Vinmec | NLP & Document AI | **Số hóa EMR và gán mã bệnh (ICD-10):** Bác sĩ tốn thời gian gõ máy tính để chuyển ghi chú lâm sàng phi cấu trúc thành dữ liệu chuẩn. Nhân viên hành chính sau đó phải đọc lại bệnh án để gán mã ICD-10 phục vụ quy trình thanh toán bảo hiểm.<br>**Tổn thất ước tính:** Bác sĩ mất **~20%** thời gian khám cho việc nhập liệu giấy tờ; sai lệch trong quá trình gán mã thủ công gây thất thoát hoặc chậm trễ xuất toán bảo hiểm y tế từ **2-4%** tổng doanh thu viện phí. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Giảm deadweight loss từ tồn kho phụ tùng  │
│ "chết" và tối ưu số ngày tồn kho mà không làm giảm SLA.     │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Quản đốc xưởng / Nhân viên cung ứng.   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Kéo data thô ──> 2. Tính TB ──> 3. Chèn thêm buffer ──>│
│   4. Đặt hàng.                                              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 - Ước lượng cảm tính│
│ (⏱ 30-45 phút/đợt đặt hàng lớn)                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Dùng thống kê ở Bước 2│
│ để tính khoảng tứ phân vị (IQR), loại bỏ các điểm dị biệt   │
│ (outliers) do nhu cầu đột biến, và tự tạo rule Min-Max chuẩn│
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Tăng tỷ lệ Vòng quay  │
│ hàng tồn kho; Giảm số ngày tồn kho từ 30 ngày ──> 20 ngày.  │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                       │
│                                                             │
│ Bài toán (1 câu): Phân luồng ưu tiên kết quả hình ảnh cho   │
│ ca cấp cứu để giảm thời gian trả kết quả (TAT) thay vì FIFO.│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ chẩn đoán hình ảnh / BN cấp cứu │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Chụp chiếu ──> 2. Đẩy vào PACs ──> 3. Bác sĩ mở file   │
│   tuần tự (FIFO) ──> 4. Đọc & Trả kết quả.                  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 - Đợi trong hàng đợi│
│ (⏱ 15-40 phút/ca chờ)                                       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Chèn hệ thống phân    │
│ luồng (Triage) ở Bước 2: Tự động đẩy ca lên đầu nếu kết hợp │
│ tuổi >60 + mã lâm sàng khẩn cấp.                            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian TAT    │
│ cho nhóm ca cấp cứu khẩn cấp từ 30 min ──> under 10 min.    │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #5                                       │
│                                                             │
│ Bài toán (1 câu): Ánh xạ mã ICD-10 tự động từ từ khóa lâm   │
│ sàng chuẩn hóa để tối đa hóa tỷ lệ duyệt chi trả bảo hiểm.  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Coder hành chính / Kế toán bảo hiểm.   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. BS gõ text tự do ──> 2. Coder đọc lại ──> 3. Tra bảng  │
│   ICD-10 thủ công ──> 4. Gán mã vào phần mềm thanh toán.    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 - Dịch free-text    │
│ sang chuẩn ICD (⏱ 5-10 phút/hồ sơ phức tạp)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Ép chuẩn hóa form nhập│
│ ở Bước 1. Dùng Regex/Elasticsearch ở Bước 3 để tự động map  │
│ keyword lâm sàng sang mã ICD chính xác 100%.                │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Tăng tỷ lệ hồ sơ duyệt│
│ chi trả lần đầu (First Pass Yield) từ 95% ──> 99.5%.        │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---
