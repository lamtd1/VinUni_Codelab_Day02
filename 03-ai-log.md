# AI Interaction Log — Reflection

**Người viết:** Dương Quốc Khánh
**Công cụ AI sử dụng:** Gemini 2.5 Flash (brainstorm & prompt prototype), Claude (cấu trúc hoá báo cáo, stress-test logic).

---

## 1. AI giúp gì cho tôi trong buổi lab?

Ở **Phase 1 (SCAN)**, tôi dùng AI như một "brainstorm partner" để liệt kê nhanh 5 bài toán tại các công ty thành viên Vingroup thay vì tự nhớ lại toàn bộ. Tôi dán prompt gợi ý trong worksheet ("Tôi là AI Engineer tại Vin Smart Future... gợi ý cho tôi 5 quy trình nghiệp vụ thủ công...") và AI trả về danh sách khá sát với `03-inspiration-kit.md`, giúp tôi tiết kiệm thời gian đọc lại toàn bộ tài liệu tham khảo và tập trung chọn lọc bài toán mình hiểu rõ nhất (điểm đón Xanh SM, phản ánh cư dân Vinhomes, tóm tắt bệnh án Vinmec).

Ở **Phase 2 (QUICK-ASSESS)**, tôi dùng prompt "đóng vai CFO/Trưởng phòng Vận hành khắt khe" để AI phản biện 3 thẻ bài toán của mình. Việc này thực sự hữu ích: AI chỉ ra rằng metric ban đầu tôi viết cho Card Vinhomes ("giảm thời gian xử lý") thiếu con số cụ thể, buộc tôi phải quay lại ước lượng thời gian từng bước (11 phút/lượt) thay vì chỉ nói chung chung.

Ở **Phase 3 (DEEP-DIVE)**, AI giúp tôi cấu trúc lại Problem Statement theo đúng 6 field và gợi ý cách trình bày Future-State Flow rõ ràng với ký hiệu AI Step / HITL / Fallback, giúp báo cáo mạch lạc hơn nhiều so với bản nháp đầu tiên tôi tự viết tay.

## 2. AI trả lời sai / hallucination ở đâu?

Hai điểm tôi phải tự sửa lại:

1. **Đề xuất kiến trúc quá phức tạp:** Ở lần brainstorm đầu, khi tôi mô tả bài toán phân loại phản ánh cư dân Vinhomes, AI đề xuất dùng **Agentic Loop** với nhiều bước gọi tool (tra cứu hồ sơ căn hộ, tự động cập nhật CRM, tự động gửi thông báo cho ban quản lý cấp cao...). Đây là over-engineering — bài toán thực chất chỉ cần phân loại + soạn draft phản hồi, không cần AI tự trị ra quyết định nhiều bước. Tôi đã tự phản biện lại theo đúng lưu ý trong `03-inspiration-kit.md` ("Problem First, AI Second") và chốt lại kiến trúc **LLM Feature** đơn giản hơn, có HITL bắt buộc.
2. **Số liệu tác động kinh doanh không có căn cứ:** Khi tôi hỏi AI ước tính "Business Impact", nó đưa ra một con số thiệt hại tài chính khá cụ thể (ví dụ mất doanh thu %) mà không có nguồn hay cơ sở nào — rõ ràng là hallucination vì AI không có dữ liệu thật của Vinhomes. Tôi đã loại bỏ con số bịa đặt đó và thay bằng ước lượng dựa trên phép tính đơn giản, có thể kiểm chứng được (số ticket/ngày × thời gian xử lý = giờ nhân lực tiêu tốn), thay vì dùng số liệu AI tự "đoán".

## 3. Tôi đã sửa prompt/ranh giới ra sao để đạt kết quả chuẩn?

* Thêm ràng buộc rõ trong prompt: *"Chỉ đề xuất kiến trúc đơn giản nhất có thể giải quyết bài toán — không tự động thêm bước Agent nếu không có yêu cầu."*
* Với các con số/metric, tôi luôn yêu cầu AI ghi rõ **giả định** đằng sau con số, và tôi tự đối chiếu giả định đó với logic thực tế (số bước × thời gian/bước) trước khi đưa vào báo cáo, thay vì copy-paste thẳng.
* Với Operational Boundary, tôi học được là phải tự viết ranh giới "CẤM" thật cụ thể (không tự gửi phản hồi, không tự đóng ticket, escalate ngay khi gặp nội dung nhạy cảm) thay vì để AI tự đề xuất chung chung, vì AI có xu hướng viết ranh giới mềm hơn mức cần thiết cho một sản phẩm chạm tới cư dân thật.

**Kết luận:** AI là công cụ tăng tốc rất tốt cho brainstorm và cấu trúc hoá tài liệu, nhưng không thể tin tưởng tuyệt đối vào số liệu định lượng hay đề xuất kiến trúc — phần việc quan trọng nhất của tôi là luôn hỏi lại "vì sao" và tự kiểm chứng trước khi đưa vào bản nộp cuối cùng.
