# 📝 AI Log — Nhật ký tương tác AI (Phase 6: Reflection)

> **Bài toán:** VinFast Warranty Claim Review — dùng LLM để tóm tắt hồ sơ claim bảo hành và draft khuyến nghị duyệt/từ chối.

## 1. AI đã giúp tôi những gì?

Trong buổi lab, tôi dùng LLM (Gemini) như một **thought-partner** ở ba giai đoạn chính:

* **Phase 1 (SCAN):** Khi mới bắt đầu, tôi chỉ nghĩ ra được 2-3 bài toán quen thuộc (chatbot CSKH, chẩn đoán lỗi xe). Tôi dùng prompt gợi ý trong worksheet để brainstorm thêm pain point ở VinFast Warranty Ops và V-Green — AI đề xuất khá nhiều ý tưởng, giúp tôi có đủ 5 bài toán nhanh hơn so với việc tự nghĩ một mình.
* **Phase 2 (Stress-test Problem Card):** Tôi dán thẻ bài toán Card #1 (Warranty Claim) vào LLM và yêu cầu nó đóng vai CFO/Trưởng phòng Vận hành khắt khe để phản biện. AI chỉ ra rằng metric ban đầu của tôi ("giảm thời gian xử lý") chưa có con số cụ thể — điều này buộc tôi phải quay lại điền số liệu thực tế (30 phút → dưới 8 phút) thay vì để chung chung.
* **Phase 4 (Prompt Prototype):** AI hỗ trợ tôi soạn thảo cấu trúc SYSTEM_PROMPT rõ ràng hơn, đặc biệt là cách diễn đạt rule "AI không được tự động duyệt claim" thành một ràng buộc output cụ thể (`status: DRAFT_RECOMMENDATION`) thay vì chỉ nói chung chung "cần con người duyệt".

## 2. AI trả lời sai / hallucination ở đâu?

* **Bịa số liệu:** Khi tôi hỏi AI ước tính số lượng claim bảo hành VinFast xử lý mỗi ngày, AI đưa ra một con số rất cụ thể (ví dụ "khoảng 500 claim/ngày trên toàn quốc") nhưng không có nguồn nào chứng minh — đây rõ ràng là số bịa ra để nghe có vẻ hợp lý. Tôi đã **không** dùng thẳng con số này vào Problem Statement; thay vào đó tôi hạ xuống một ước tính thận trọng hơn (120 claim/ngày) và ghi rõ đây là giả định của nhóm, không phải số liệu thật từ VinFast.
* **Đề xuất kiến trúc quá phức tạp:** Ở bước AI-Fit, AI ban đầu gợi ý dùng **Agentic Loop** (agent tự tra cứu hệ thống, tự gọi API chính sách bảo hành, tự gửi email cho đại lý) vì nghe "hiện đại" hơn. Nhưng khi tôi hỏi lại "rủi ro tài chính khi agent tự quyết định sai thì sao?", AI mới thừa nhận với bài toán có rủi ro tài chính/pháp lý cao và cần con người duyệt 100%, **LLM Feature + HITL bắt buộc** là lựa chọn an toàn và đơn giản hơn nhiều so với Agent tự trị. Đây là ví dụ AI có xu hướng đề xuất giải pháp phức tạp hơn mức cần thiết nếu không bị chất vấn.
* **Bỏ sót ranh giới an toàn:** Ở lần đầu viết Operational Boundary, AI chỉ đưa ra một câu rất chung "AI cần được con người giám sát". Tôi phải hỏi lại cụ thể "Nếu dữ liệu ảnh/mô tả không rõ ràng thì AI phải làm gì?" thì AI mới bổ sung được cơ chế Fallback (chuyển về xử lý thủ công khi confidence thấp) — nếu không hỏi thêm, ranh giới sẽ có lỗ hổng.

## 3. Tôi đã sửa prompt / đặt ranh giới như thế nào?

* Thay vì hỏi chung chung "gợi ý bài toán AI cho VinFast", tôi thu hẹp prompt lại theo đúng 4 Lenses trong worksheet (Repetitive/Time-consuming/AI-upgrade/Stakeholder Pain) để câu trả lời bám sát cấu trúc bài, tránh AI trả lời lan man.
* Với mọi con số AI đưa ra (thời gian xử lý, số lượng claim/ngày), tôi coi đó là **giả thuyết cần kiểm chứng**, không phải sự thật — tôi luôn tự hỏi lại "con số này AI lấy từ đâu?" trước khi đưa vào báo cáo, và chủ động làm tròn/hạ thấp để tránh phóng đại impact.
* Khi thiết kế Operational Boundary trong prompt_prototype.py, tôi yêu cầu AI viết cả **test case tấn công (adversarial input)** cố tình dụ hệ thống bỏ qua bước duyệt của con người (ví dụ viện lý do "khách VIP", "gấp"). Việc bắt AI tự tấn công chính ranh giới nó đề xuất giúp tôi phát hiện những câu lệnh chưa đủ chặt (ví dụ ban đầu rule chỉ nói "cần duyệt" mà không cấm rõ output ở trạng thái `APPROVED`).

## 4. Bài học rút ra

AI là một công cụ brainstorm và phản biện rất tốt để tăng tốc độ scoping, nhưng **không thể tin tưởng mù quáng vào số liệu hay kiến trúc nó đề xuất đầu tiên**. Vai trò của tôi là người đặt câu hỏi ngược lại (stress-test), kiểm chứng số liệu, và luôn ưu tiên giải pháp đơn giản nhất giải quyết được bài toán thay vì giải pháp "nghe hoành tráng" mà AI gợi ý ban đầu.
