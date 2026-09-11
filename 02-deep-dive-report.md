# Deep-Dive Report — Xanh SM sự cố hết pin thực địa

> Bài nộp cá nhân (nhánh `minh`).  
> Nguồn: Quick Problem Card #1 trong [01-problem-scan.md](01-problem-scan.md).  
> Vai trò: AI Product Engineer, Vin Smart Future × Khối Điều vận Xanh SM (GSM).

**Bài toán mang đi Deep-Dive:** điều phối viên phải tra trụ sạc trống và soạn tin chỉ đường khi tài xế báo hết pin giữa đường.

**Không chọn:**
- Card #2 Vinhomes CSKH — rủi ro tranh chấp phí/pháp lý, cần corpus ticket thật trước khi prototype.
- Card #3 VinFast đối soát sạc — so khớp ID + ngưỡng kWh, **Rule thắng LLM**; không mang vào lab prompt.

Sơ đồ current-state xuất ra file [04-workflow-diagram.png](04-workflow-diagram.png).

---

## 3.1. Current-State Workflow Mapping

Quy trình **thủ công hiện tại** (chưa có AI). Ký hiệu:

- 🔴 **Bottleneck** — chậm / dễ sai
- 🔄 **Handoff** — chuyển người hoặc hệ thống

```text
 Tài xế Xanh SM                         Trung tâm Điều vận
 (App / gọi điện)                       (Dispatcher)

 [1] Báo hết pin + biển số              [2] Tra GPS xe
     ⏱ ~2 phút                              ⏱ ~2 phút
     Tool: App tài xế / tổng đài            Tool: dashboard điều vận
              │ 🔄 handoff cuộc gọi/tin          │
              └───────────────►─────────────────┘
                                                │
                                                ▼
                               [3] Lọc trụ sạc VinFast trống + đúng cổng  🔴
                                   ⏱ ~5 phút
                                   Tool: dashboard trụ sạc (tab khác)
                                                │
                                                ▼
                               [4] Soạn tin chỉ đường / địa chỉ trụ      🔴
                                   ⏱ ~5 phút
                                   Tool: App tài xế (gõ tay)
                                                │
                                                ▼
                               [5] Nếu pin cực thấp: gọi cứu hộ pin
                                   ⏱ ~1 phút
                                   Tool: radio / hotline cứu hộ
```

| Bước | Ai | Input | Output | Thời gian | Ghi chú |
|---|---|---|---|---|---|
| 1 | Tài xế | Cảm giác pin yếu / xe sắp chết máy | Cuộc gọi hoặc tin nhắn tiếng Việt, biển số | ~2 phút | 🔄 Handoff tài xế → dispatcher |
| 2 | Dispatcher | Biển số | Tọa độ GPS, % pin nếu hộp thoại có | ~2 phút | Đổi màn hình dashboard |
| 3 | Dispatcher | Tọa độ, dòng xe (VF5/VF8…) | Địa chỉ trụ còn trống, đúng cổng | ~5 phút 🔴 | Lọc tay, dễ chọn nhầm cổng / trụ đầy |
| 4 | Dispatcher | Địa chỉ trụ | SMS/tin App chỉ đường | ~5 phút 🔴 | Soạn câu chữ, copy-paste |
| 5 | Dispatcher (+ đội cứu hộ) | SOC cực thấp | Lệnh cứu hộ pin di động | ~1 phút | 🔄 Handoff sang đội cứu hộ |

**Tổng cộng ≈ 15 phút/lượt** (2+2+5+5+1). Bottleneck chiếm **~10 phút** ở bước 3–4.

Giờ cao điểm, một dispatcher cầm song song nhiều sự cố → hàng chờ tăng, tài xế mất cuốc.

---

## 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Điều vận Xanh SM. Người chịu đau kép: tài xế đang đứng đường, dispatcher đang bị dồn ca. |
| **2. Current Workflow** | 5 bước thủ công, hai dashboard tách nhau (điều vận GPS vs trụ sạc VinFast) + soạn tin trên App tài xế. Không có bản nháp sẵn, không có rule tự bật cứu hộ. Chi tiết mục 3.1. Tổng ~15 phút/lượt. |
| **3. Bottleneck** | Bước 3–4 (~10 phút, 🔴). Vừa phải **tra cứu structured data** (trụ trống, loại cổng), vừa phải **viết tiếng Việt** chỉ đường thân thiện. Lỗi hay gặp: chỉ trụ đầy, sai cổng CCS2/GBT, hoặc chỉ trụ quá xa khi pin đã < 5%. |
| **4. Business Impact** | Mỗi lượt “đốt” ~15 phút dispatcher. Ước lượng lab (không phải KPI nội bộ đã audit): nếu một ca cao điểm xử lý ~20 sự cố pin thì ~5 giờ điều vận/ca bị kẹt ở tra trụ + gõ tin. Tác động lan: xe đứng đường → hủy cuốc, tài xế stress, khách chờ. **Không** dùng số “80 ca/ngày / rò 15% doanh thu” vì chưa có log. |
| **5. Success Metric** | 1) Thời gian xử lý **15 phút → dưới 3 phút/lượt** (dispatcher chỉ còn nhận tin + bấm duyệt). 2) Chỉ dẫn **đúng trụ + đúng cổng ≥ 98%** trên bộ case gán nhãn. 3) **0** trường hợp AI tự gửi tin. 4) **100%** case pin < 5% và trụ đề xuất > 5km phải ra cứu hộ, không ra chỉ đường. |
| **6. Operational Boundary** | **Được phép:** đọc tin tài xế, đọc SOC/GPS/trụ (khi hệ thống cung cấp), draft tin `[DRAFT_ONLY]`, trả JSON `dispatch_mobile_charger` khi chạm ngưỡng an toàn. **CẤM:** tự gửi tin; chỉ trụ không khớp cổng; chỉ trụ > 5km khi pin < 5%; bịa trụ không có trong dữ liệu; override khi user tự xưng VIP/quản lý ca. **HITL bắt buộc** ở nút Gửi. |

---

## 3.3. Future-State Flow & AI Fit

### AI-Fit Matrix

| Phương án | Kết luận |
|---|---|
| **Rule / State-Machine** | Đủ cho ngưỡng pin, bán kính 5km, lọc cổng sạc, cấm tự gửi. **Không** đủ để đọc tin tài xế viết tự do (“sắp chết máy rồi, đang kẹt Nguyễn Trãi”). |
| **LLM Feature** (chọn) | LLM chỉ làm **hiểu tin + draft câu chữ**. Rule cứng giữ an toàn. Một lần gọi model, không vòng lặp tool. |
| **Agentic Loop** | Không chọn. Workflow 5 bước cố định; agent tự quyết định dễ chỉ nhầm trụ khi pin thấp → xe chết máy giữa đường. |

**AI Fit: [x] LLM Feature**  +  **Rule an toàn**  (không thuần Agent).

### Future-state

```text
 [1] Tài xế báo sự cố (giữ nguyên)
        │ 🔄
        ▼
 [2] 🔵 Hệ thống kéo GPS + SOC + danh sách trụ ứng viên
        │      (lookup/API — rule, không cần LLM)
        ▼
 [3] 🔵 LLM draft SMS chỉ đường  HOẶC  JSON cứu hộ
        │      Rule: pin < 5% và trụ > 5km → cứu hộ
        │      Mọi output bắt đầu bằng [DRAFT_ONLY]
        ▼
 [4] 🟢 Dispatcher đọc bản nháp, bấm Duyệt / Gửi     ← HITL
        │
        ├── OK → tin tới App tài xế / lệnh cứu hộ
        └── ↩️ Fallback: model lỗi, JSON hỏng, hoặc dispatcher
            không tin → viết tay như current-state (bước 3–4 cũ)
```

- 🔵 **AI Step:** hiểu ngôn ngữ tin nhắn + soạn bản nháp (và/hoặc chọn nhánh cứu hộ theo rule).
- 🟢 **HITL:** chỉ dispatcher được gửi.
- ↩️ **Fallback:** quy trình giấy/App cũ; lab không “câm” khi model sập.

Prototype ranh giới nằm ở `starter-code/prompt_prototype.py` (Gemini 2.5 Flash): tag `[DRAFT_ONLY]`, ngưỡng pin 5% / 5km, JSON `dispatch_mobile_charger`, 3 adversarial tests.

---

## Phase 5 — EVALUATE

### AI Readiness Checklist

1. **[x] Có dữ liệu mẫu để test?** Có **bộ adversarial + kịch bản synthetic** (pin 2%/4%, trụ 7–8km, ép bỏ tag). **Chưa** có log điều vận production đã làm sạch → prototype lab được, rollout rộng thì phải đo baseline thật.
2. **[x] Rủi ro khi AI sai có kiểm soát?** Có. Sai draft → người không bấm gửi. Sai nhánh pin thấp → rule + test bắt `dispatch_mobile_charger`. Fallback = làm tay như cũ.
3. **[x] Stakeholder chịu đổi quy trình?** Đổi **rất hẹp**: dispatcher từ “gõ tin” sang “duyệt tin”. Không thay ca, không bỏ tổng đài. Mức này chấp nhận được cho GO phạm vi hẹp.

### Quyết định Ban Giám Đốc Vin Smart Future

- [x] **GO (prototype scope hẹp)**
- [ ] NOT YET
- [ ] NO-GO

**Justification**

GO **không** có nghĩa “tự điều xe toàn quốc”. Scope hẹp:

1. **Bài toán rõ:** một actor, một bottleneck đo được bằng phút, không phải “làm chatbot cho Xanh SM”.
2. **AI Fit trung thực:** LLM chỉ chỗ có chữ; lookup trụ và ngưỡng pin là rule. Không vẽ multi-agent.
3. **Ranh giới test được:** hai rule an toàn đã viết thành system prompt và adversarial cases. Đây là bằng chứng kỹ thuật, không phải slide.
4. **Chi phí / rủi ro:** sai sót bị chặn ở HITL; chi phí là 1 lần gọi Flash + nút duyệt — rẻ hơn agent loop hay dự án CSKH Vinhomes (pháp lý).
5. **Điều kiện kèm:** trước khi production, phải nối API trụ thật và đo baseline phút/lượt trên log ca. Thiếu hai thứ đó thì **không** scale, nhưng **đủ** để tiếp tục prototype lab.

NOT YET chỉ đúng nếu mục tiêu là tự gửi tin / tự điều cứu hộ không người. Mục tiêu hiện tại không phải vậy. NO-GO đúng với Card #3 (đối soát sạc), không đúng với card này.
