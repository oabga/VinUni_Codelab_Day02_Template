# 03-ai-log.md: Nhật ký Tương tác AI (AI Interaction Log)

**Người thực hiện:** AI Engineer / Chuyên viên Tối ưu Vận hành
**Dự án:** Ứng dụng AI tối ưu hóa vận hành hệ sinh thái Vin Smart Future

---

## 1. AI đã giúp tôi những gì?
Trong quá trình lên ý tưởng và đóng gói giải pháp, các trợ lý AI (như ChatGPT, Gemini) đã đóng vai trò là một "sparring partner" (đối tác phản biện) đắc lực:
*   **Hệ thống hóa ý tưởng:** Giúp tôi chuyển đổi các vấn đề vận hành rời rạc thành cấu trúc Problem Card chuẩn chỉnh và các ma trận AI-Fit rõ ràng.
*   **Phác thảo giải pháp kỹ thuật:** Cung cấp các nền tảng kiến trúc ban đầu (Agentic Workflow, RAG, Tool calling) để giải quyết bài toán phân loại ticket khiếu nại và tự động hóa quy trình.
*   **Thiết kế trực quan:** Tôi đã sử dụng AI để tạo ra các hình minh họa dạng vector phẳng tối giản (minimalist flat vector illustrations), giúp các slide thuyết trình báo cáo và sơ đồ luồng nghiệp vụ trở nên chuyên nghiệp và dễ tiếp cận hơn đối với Ban Giám đốc.

## 2. Những điểm AI trả lời sai (Hallucination) và Thiếu sót
Dù rất hữu ích, AI không ít lần mắc lỗi tư duy logic, đặc biệt trong các bài toán đòi hỏi độ chính xác tuyệt đối về thống kê và số liệu thực tế:
*   **Sai lệch thuật toán thống kê cơ bản:** Khi tôi yêu cầu xây dựng rule-based để phát hiện các điểm dị biệt (outliers) trong dữ liệu tồn kho và pin BMS, AI đã xác định sai phương pháp tính toán vị trí của tứ phân vị thứ nhất (Q1). Điều này dẫn đến việc tính toán khoảng tứ phân vị (IQR) bị lệch hoàn toàn, khiến hệ thống nhận diện sai các tín hiệu bất thường.
*   **Bịa đặt số liệu vĩ mô:** Trong một bối cảnh yêu cầu phân tích các tác động kinh tế đến thị trường, AI đã gặp hiện tượng hallucination khi tự tin đưa ra tỷ lệ thất nghiệp tự nhiên (Natural Rate of Unemployment) là 5.6%. Thực tế, con số chính xác mà tôi phải rà soát và đính chính lại là 6.6%.
*   **Lan man trong logic vận hành:** Khi thảo luận về bài toán điều vận của Xanh SM Express, ban đầu AI đưa ra các giải pháp logistics rất chung chung và phi thực tế. Tôi đã phải dùng những kiến thức cốt lõi từ các nghiên cứu chuyên sâu về Last-Mile Delivery in E-Commerce để kéo AI về đúng trọng tâm: giải quyết nút thắt trong trải nghiệm khách hàng và tối ưu hóa chi phí chặng cuối.

## 3. Quá trình tinh chỉnh Prompt và Thiết lập ranh giới (Boundaries)
Để khắc phục các điểm yếu trên, tôi đã thay đổi chiến lược giao tiếp với AI:
*   **Gắn chặt ranh giới kỹ thuật (Technical Grounding):** Thay vì hỏi mở "Làm sao để phát hiện lỗi", tôi ép AI vào framework cụ thể bằng prompt: *"Hãy dùng phương pháp IQR, lưu ý tính chính xác vị trí Q1 theo công thức chuẩn để tìm outliers, tuyệt đối không dùng Machine Learning cho bước này"*.
*   **Áp dụng Zero-Tolerance với số liệu:** Bổ sung các chỉ thị kiểm soát gắt gao như *"Chỉ cung cấp architecture flow, không tự ý chèn các chỉ số kinh tế vĩ mô nếu không được cung cấp"* hoặc yêu cầu AI xác nhận lại các hằng số trước khi đưa vào phân tích.
*   **Đóng vai (Role-prompting) khắt khe:** Tôi tạo ra một persona "CFO/Trưởng phòng Vận hành khó tính" để ép AI phải tự phản biện lại chính giải pháp của nó. Điều này giúp loại bỏ các đề xuất dùng AI lãng phí, thay vào đó là các giải pháp Rule-based hiệu quả và tiết kiệm chi phí hơn.