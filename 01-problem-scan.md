# Phase 1–2 — Problem Scan & Quick Cards (Cá nhân)

> Vai trò: AI Product Engineer tại **Vin Smart Future**.  
> Mục tiêu: quét pain point vận hành ở các công ty thành viên Vingroup, rồi đóng 3 thẻ bài toán đủ rõ để nhóm chọn 1 cái đi Deep-Dive.

---

## Phase 1 — SCAN (5 bài toán, 4 lenses)

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Điều phối viên xử lý sự cố hết pin thực địa: tra cứu vị trí xe, tìm trụ sạc VinFast còn trống đúng loại cổng, rồi soạn tin chỉ đường gửi tài xế — mất khoảng 12–15 phút/lượt vào giờ cao điểm. |
| 2 | **VinFast** | Lặp lại | Mỗi tuần kế toán dịch vụ phải so khớp hàng nghìn phiên sạc roaming (trụ đối tác) với hóa đơn PDF/CSV: lệch số kWh, lệch thời điểm, lệch mã trụ — làm tay trên Excel. |
| 3 | **Vinhomes** | AI-upgrade | CSKH Ban Quản Lý soạn phản hồi khiếu nại cư dân trên App Vinhomes Resident (mất nước, ồn, phí xe, thi công) theo mẫu rập khuôn; SLA cam kết 12 giờ nhưng giờ cao điểm trễ 18–24 giờ. |
| 4 | **Vinmec** | Pain từ người khác | Bác sĩ điều trị phải tự viết tóm tắt xuất viện từ ghi chú EMR, kết quả xét nghiệm và toa thuốc rời rạc — 20–30 phút/ca; bác sĩ phàn nàn vì cắt giờ khám. |
| 5 | **Vinpearl** | Lặp lại | Lễ tân / sales đọc email đặt phòng theo đoàn (tiếng Việt lẫn tiếng Anh), copy thông tin sang PMS để kiểm quỹ phòng rồi draft thư xác nhận — 1 email đoàn mất 20–40 phút. |

**Ghi chú lựa chọn lens:** 5 bài phủ đủ 4 lenses (Tốn thời gian, Lặp lại, AI-upgrade, Pain). Không dồn hết vào 1 công ty.

---

## Phase 2 — QUICK-ASSESS (top 3 cards)

Chọn **#1 (Xanh SM sự cố pin)**, **#3 (Vinhomes CSKH)**, **#2 (VinFast đối soát sạc)**.

Lý do loại #4 Vinmec: rủi ro lâm sàng cao, bắt buộc bác sĩ ký — phù hợp làm sau khi đã có HITL chặt, không phải bài prototype 30 phút.  
Lý do loại #5 Vinpearl: phụ thuộc dữ liệu quỹ phòng PMS; thiếu API thì AI chỉ draft được chữ, không kiểm được phòng trống.

---

### Quick Problem Card #1 — Xanh SM sự cố hết pin thực địa

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tài xế Xanh SM báo hết pin / sự cố sạc    │
│ giữa đường; điều phối phải tìm trụ trống và soạn chỉ dẫn.   │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   - Điều phối viên (quá tải giờ cao điểm)                   │
│   - Tài xế (chờ, mất cuốc)                                  │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Tài xế gọi/nhắn tổng đài báo hết pin + biển số         │
│   2. Dispatcher tra GPS xe trên dashboard điều vận          │
│   3. Mở dashboard trụ sạc VinFast, lọc trụ trống + đúng cổng│
│   4. Soạn tin chỉ đường / địa chỉ trụ gửi App tài xế        │
│   5. Nếu pin cực thấp: gọi đội cứu hộ pin di động           │
│                                                             │
│ Bước tốn nhất: Bước 3–4 (⏱ ~12 phút/lượt)                   │
│ AI nhảy vào: Bước 3–4 (gợi ý trụ / draft tin) + cờ cứu hộ   │
│              khi pin < 5%. Người vẫn bấm gửi.               │
│                                                             │
│ Metric có số:                                               │
│   Thời gian xử lý 15 phút ──> dưới 3 phút/lượt              │
│   Tỉ lệ chỉ đúng trụ (đúng cổng, còn trống) >= 98%          │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│   LLM Feature: đọc tin tài xế tiếng Việt, draft SMS.        │
│   Rule cứng: pin < 5% và trụ > 5km → cứu hộ, không chỉ đường│
└─────────────────────────────────────────────────────────────┘
```

**Vì sao LLM chứ không phải Agent?** Quy trình cố định (GPS → trụ → draft). Agent tự gọi tool vòng lặp không cần thiết; sai trụ khi pin thấp thì xe chết máy giữa đường.

---

### Quick Problem Card #2 — Vinhomes phản hồi khiếu nại cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): CSKH Ban Quản Lý soạn tay phản hồi        │
│ khiếu nại cư dân trên App; chậm và hay trả lời chung chung. │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   - Nhân viên CSKH BQL (soạn 8–12 phút/ticket)              │
│   - Cư dân (chờ SLA, ticket bị route sai tòa)               │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Cư dân gửi ticket (text + ảnh) trên App Resident       │
│   2. CSKH đọc, đoán loại (nước / điện / ồn / phí / thi công)│
│   3. Forward tay sang kỹ thuật tòa hoặc kế toán             │
│   4. Soạn phản hồi theo template Word, copy vào App         │
│   5. Supervisor duyệt với ticket liên quan phí/tranh chấp   │
│                                                             │
│ Bước tốn nhất: Bước 2 + 4 (⏱ ~10 phút/ticket; lỗi route ~15%)│
│ AI nhảy vào: Bước 2 (phân loại + tòa) và Bước 4 (draft).    │
│              Bước 5 giữ HITL với ticket phí / pháp lý.      │
│                                                             │
│ Metric có số:                                               │
│   Thời gian soạn phản hồi 10 phút ──> dưới 2 phút           │
│   Ticket được phân đúng loại + đúng tòa >= 90%              │
│   Không ticket phí nào được gửi khi chưa có người duyệt     │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│   LLM phân loại ngôn ngữ tự nhiên + draft.                  │
│   Rule router (tòa nhà, mã căn) vẫn làm phần định tuyến.    │
└─────────────────────────────────────────────────────────────┘
```

**Stress-test (CFO / Ops):** Keyword rule (`nước`, `ồn`) bắt được ~60–70% ticket ngắn, nhưng trượt ticket kiểu *"ban công nhà trên xả đồ ướt xuống"* (không có từ khóa). LLM hợp lý ở bước đọc chữ; **không** để LLM tự gửi vì tranh chấp phí quản lý có rủi ro pháp lý.

---

### Quick Problem Card #3 — VinFast đối soát phiên sạc roaming

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Kế toán dịch vụ so khớp tay phiên sạc     │
│ roaming đối tác với hóa đơn tuần; lệch kWh / mã trụ / giờ.  │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   - Kế toán dịch vụ sạc (2–3 ngày/tuần cắm Excel)           │
│   - Đối tác trụ sạc (chậm đối soát, chậm thanh toán)        │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Export log phiên sạc từ hệ thống VinFast (CSV)         │
│   2. Nhận hóa đơn đối tác (PDF/Excel)                       │
│   3. Vlookup mã trụ + timestamp trên Excel                  │
│   4. Flag dòng lệch > 0.5 kWh hoặc lệch giờ > 5 phút        │
│   5. Gọi/email đối tác để giải trình dòng lệch              │
│                                                             │
│ Bước tốn nhất: Bước 3–4 (⏱ ~8 phút/lô 50 dòng; cả tuần ~12h)│
│ AI nhảy vào: Không cần LLM cho so khớp ID. Có thể LLM đọc   │
│              PDF hóa đơn scan xấu (bước 2) nếu OCR fail.    │
│                                                             │
│ Metric có số:                                               │
│   Thời gian đối soát tuần 12 giờ ──> dưới 2 giờ             │
│   Bắt 100% dòng lệch kWh > 0.5 so với rule hiện tại         │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
│   Join theo (station_id, start_ts ±5 phút, kWh).            │
│   LLM chỉ là bước phụ nếu hóa đơn là ảnh/PDF không structured│
└─────────────────────────────────────────────────────────────┘
```

**Vì sao Rule, không phải LLM?** Đây là so khớp khóa + ngưỡng số. LLM dễ “xấp xỉ” kWh — kế toán không chấp nhận. Giữ bài này trong top 3 để chứng minh **không nhồi AI vào mọi chỗ**.

---

## Đề xuất mang sang nhóm (Phase 3)

Ưu tiên **Card #1 — Xanh SM sự cố hết pin** cho Deep-Dive + prompt prototype:

- Có actor, bottleneck phút, metric trước/sau, ranh giới an toàn rõ (`[DRAFT_ONLY]`, pin < 5% → cứu hộ).
- Starter code lab đã gắn đúng use case này, stress-test được trong 30 phút.
- Card #2 để dự phòng nếu nhóm muốn CSKH. Card #3 không mang đi prototype LLM.
