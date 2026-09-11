# Phase 1 — Problem Scan

## Problem Table

| # | Subsidiary | Lens | Problem |
| --- | --- | --- | --- |
| 1 | Xanh SM | Time-consuming | Dispatcher manually assesses whether an EV has sufficient battery for the next trip |
| 2 | Xanh SM | Stakeholder Pain | Manual analysis of trip cancellation reasons |
| 3 | VinFast | Repetitive | Manual routing of customer vehicle fault descriptions |
| 4 | Vinhomes | AI-upgrade | Manual classification of resident complaints |
| 5 | Vinpearl | Stakeholder Pain | Manual detection of urgent negative guest reviews |

## Quick Card 1 — Xanh SM Battery Risk

```text
┌───────────────────────────────────────────────────────────────┐
│                                                               │
│ QUICK PROBLEM CARD #1                                         │
│                                                               │
│ Bài toán (1 câu): Dispatcher phải đánh giá xem xe điện có     │
│   đủ pin để hoàn thành cuốc tiếp theo an toàn.                │
│ Công ty thành viên: [ ] VinFast  [X] Xanh SM  [ ] Vinhomes    │
│                     [ ] Vinmec   [ ] Khác (ghi rõ) ___        │
│                                                               │
│ Ai đang đau (Actor)? Xanh SM Dispatcher                       │
│                                                               │
│ Workflow thủ công hiện tại (3-5 bước):                        │
│   1. Nhận cuốc ──> 2. Tìm xe trống ──> 3. Check pin ──>       │
│   4. Check distance ──> 5. Estimate risk ──> 6. Assign        │
│ Bước nào tốn thời gian/lỗi nhất? Battery-risk assessment      │
│   (⏱ ~2–4 phút/lượt; giả định prototype, cần kiểm chứng)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 5: tóm tắt rủi     │
│   ro và sinh khuyến nghị cho dispatcher                       │
│                                                               │
│ Đo thành công bằng gì (Metric có số)? Decision-support time   │
│   ──> dưới 30 giây (từ ~2–4 phút)                             │
│                                                               │
│ Quick Architecture: [ ] No AI  [X] Rule  [X] LLM  [ ] Agent   │
│   (+ Human-in-the-loop phê duyệt)                             │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

---

## Quick Card 2 — VinFast Fault Routing

```text
┌───────────────────────────────────────────────────────────────┐
│                                                               │
│ QUICK PROBLEM CARD #2                                         │
│                                                               │
│ Bài toán (1 câu): Customer fault descriptions must be         │
│   manually interpreted and routed to the right team.          │
│ Công ty thành viên: [X] VinFast  [ ] Xanh SM  [ ] Vinhomes    │
│                     [ ] Vinmec   [ ] Khác (ghi rõ) ___        │
│                                                               │
│ Ai đang đau (Actor)? Customer service / service advisor       │
│                                                               │
│ Workflow thủ công hiện tại (3-5 bước):                        │
│   1. Nhận khiếu nại ──> 2. Đọc mô tả ──> 3. Suy luận vấn đề   │
│   ──> 4. Chọn hạng mục ──> 5. Định tuyến kỹ thuật             │
│ Bước nào tốn thời gian/lỗi nhất? Diễn giải ngôn ngữ tự nhiên  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Classify complaint +    │
│   giải thích kết quả phân loại                                │
│                                                               │
│ Đo thành công bằng gì (Metric có số)? >=90% định tuyến đúng   │
│   trên bộ dữ liệu kiểm thử có nhãn                            │
│                                                               │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [ ] Agent   │
│   (+ Human Review)                                            │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

---

## Quick Card 3 — Vinhomes Complaint Routing

```text
┌───────────────────────────────────────────────────────────────┐
│                                                               │
│ QUICK PROBLEM CARD #3                                         │
│                                                               │
│ Bài toán (1 câu): Resident complaints need manual             │
│   classification and routing to building-management teams.    │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [X] Vinhomes    │
│                     [ ] Vinmec   [ ] Khác (ghi rõ) ___        │
│                                                               │
│ Ai đang đau (Actor)? Customer service staff                   │
│                                                               │
│ Workflow thủ công hiện tại (3-5 bước):                        │
│   1. Nhận khiếu nại ──> 2. Nhân viên đọc ──> 3. Xác định      │
│   hạng mục ──> 4. Phòng ban phù hợp ──> 5. Chuyển tiếp        │
│ Bước nào tốn thời gian/lỗi nhất? Phân loại khiếu nại          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Classify + suggest      │
│   route                                                       │
│                                                               │
│ Đo thành công bằng gì (Metric có số)? >=90% độ chính xác      │
│   định tuyến                                                  │
│                                                               │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [ ] Agent   │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

---

## Selected Problem

The selected problem is **BatteryGuard AI — Pre-Dispatch Battery Risk Assessment for Xanh SM**.

This problem was selected because:

- it has a clear operational actor;
- the inputs can be represented as structured data;
- safety boundaries can be clearly defined;
- success can be measured quantitatively;
- deterministic rules and LLM capabilities can be meaningfully separated.
