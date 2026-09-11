# 01 — Problem Scan & Quick Problem Cards

## Bối cảnh

Vin Smart Future tìm kiếm các cơ hội AI có thể cải thiện hiệu suất vận hành tại các công ty thành viên Vingroup. Các vấn đề dưới đây được chọn theo bốn lăng kính: công việc lặp lại, tốn thời gian, AI có thể nâng cấp và nỗi đau của stakeholder.

## Phase 1 — SCAN

| # | Công ty | Lens | Vấn đề / bottleneck | Cơ hội cải thiện |
|---:|---|---|---|---|
| 1 | Xanh SM | Tốn thời gian / Stakeholder Pain | Tài xế báo sự cố sạc hoặc hết pin giữa đường; dispatcher phải tra GPS, tìm trạm còn chỗ rồi soạn hướng dẫn thủ công. | Tự động lấy dữ liệu vị trí, lọc trạm phù hợp và tạo draft điều phối an toàn. |
| 2 | Vinhomes | Lặp lại / AI-upgrade | CSKH phải đọc, phân loại và chuyển các phản ánh của cư dân qua nhiều nhóm quản lý tòa nhà. | Phân loại nội dung, trích xuất thông tin và route đúng bộ phận. |
| 3 | Vinmec | Tốn thời gian | Bác sĩ mất nhiều thời gian tổng hợp bệnh án, kết quả xét nghiệm và ghi chú để viết tóm tắt xuất viện. | Trích xuất và tạo bản nháp tóm tắt để bác sĩ kiểm tra, ký duyệt. |
| 4 | VinFast | Lặp lại | Nhân viên phải đối chiếu hàng nghìn bản ghi sạc với hóa đơn đối tác theo tuần. | Phát hiện bản ghi lệch, gom nhóm ngoại lệ và tạo báo cáo đối chiếu. |
| 5 | Xanh SM | Nỗi đau stakeholder | Doanh nghiệp có nhiều cuộc hủy chuyến nhưng lý do nằm rải rác trong ghi âm và ghi chú của tài xế. | Phân loại nguyên nhân hủy chuyến và tìm các pattern theo khu vực/thời gian. |
| 6 | Vinpearl | AI-upgrade / Nỗi đau stakeholder | Review khách sạn trên nhiều kênh chưa được tổng hợp kịp thời để quản lý xử lý phàn nàn nghiêm trọng. | Gom review, phân loại chủ đề và cảnh báo vấn đề cần ưu tiên. |

## Phase 2 — QUICK-ASSESS

### Quick Problem Card #2 — Xanh SM xử lý sự cố pin

- **Bài toán:** Tài xế Xanh SM báo sự cố sạc hoặc hết pin giữa đường và cần được điều phối đến phương án cứu hộ an toàn.
- **Actor đang gặp vấn đề:** Tài xế phải chờ; dispatcher bị quá tải trong giờ cao điểm.
- **Workflow hiện tại:**
  1. Tài xế gọi tổng đài báo sự cố.
  2. Dispatcher tra vị trí xe trên bản đồ nội bộ.
  3. Dispatcher tra các trạm VinFast còn trụ trống và loại cổng phù hợp.
  4. Dispatcher tự viết tin nhắn chỉ dẫn rồi gửi qua ứng dụng tài xế.
  5. Dispatcher liên hệ đội cứu hộ nếu xe đã cạn pin.
- **Bước tốn thời gian/lỗi nhất:** Bước 3–4, khoảng 12 phút/lượt; dễ chọn nhầm trạm, khoảng cách hoặc loại cổng sạc.
- **AI hỗ trợ:** LLM Feature tạo draft hướng dẫn sau khi dữ liệu GPS, trạm sạc, khoảng cách và loại cổng đã được hệ thống kiểm tra.
- **Metric:** Giảm thời gian xử lý từ 15 phút xuống dưới 3 phút/lượt; đạt ít nhất 98% hướng dẫn đúng địa điểm và đúng loại cổng.
- **Quick Architecture:** Rule/state-machine cho các điều kiện an toàn + LLM Feature cho việc soạn ngôn ngữ.
- **Boundary:** Nếu pin dưới 5%, không chỉ dẫn trạm tiêu chuẩn xa hơn 5 km; phải draft lệnh `dispatch_mobile_charger`. Mọi tin nhắn phải bắt đầu bằng `[DRAFT_ONLY]` và chờ dispatcher duyệt.

### Quick Problem Card #4 — Vinhomes phân loại phản ánh cư dân

- **Bài toán:** Phân loại và chuyển phản ánh của cư dân đến đúng ban quản lý/tòa nhà.
- **Actor đang gặp vấn đề:** Nhân viên CSKH và cư dân chờ phản hồi.
- **Workflow hiện tại:** Cư dân gửi phản ánh → CSKH đọc nội dung → tra danh mục → chuyển thủ công → bộ phận phụ trách phản hồi.
- **Bước tốn thời gian/lỗi nhất:** Đọc và phân loại các phản ánh tự do ngôn ngữ; mục tiêu hiện tại khoảng 12 giờ để có phản hồi ban đầu.
- **AI hỗ trợ:** Phân loại chủ đề, trích xuất tòa nhà/căn hộ và tạo draft phản hồi, nhưng không tự trả lời các vấn đề pháp lý, phí quản lý hoặc tranh chấp.
- **Metric:** 85% phản ánh được phân loại trong 10 giây; giảm phản hồi ban đầu từ 12 giờ xuống dưới 2 giờ trong phạm vi thử nghiệm.
- **Quick Architecture:** Rule-based router kết hợp LLM Feature có kiểm duyệt.
- **Boundary:** Không tự kết luận trách nhiệm, chi phí hoặc quyền lợi pháp lý; các trường hợp nhạy cảm phải chuyển người phụ trách.

### Quick Problem Card #6 — Xanh SM phân tích lý do hủy chuyến

- **Bài toán:** Tóm tắt lý do khách hủy chuyến từ ghi âm cuộc gọi và ghi chú của tài xế để tìm nguyên nhân lặp lại.
- **Actor đang gặp vấn đề:** Nhóm vận hành và phân tích dữ liệu phải nghe/đọc thủ công nhiều bản ghi.
- **Workflow hiện tại:** Thu thập ghi âm → nghe và đọc ghi chú → gắn nhãn lý do → tổng hợp bảng → phân tích theo ngày/khu vực.
- **Bước tốn thời gian/lỗi nhất:** Nghe và gắn nhãn thủ công; mục tiêu xử lý hiện tại khoảng 10 phút/bản ghi.
- **AI hỗ trợ:** Chuyển giọng nói thành văn bản, tóm tắt và đề xuất nhãn nguyên nhân để analyst review.
- **Metric:** 90% bản ghi được gắn nhãn trong dưới 30 giây; độ chính xác nhãn sau review đạt ít nhất 90%.
- **Quick Architecture:** LLM Feature cho tóm tắt/phân loại offline; không tự động thay đổi chính sách vận hành.
- **Boundary:** Ẩn thông tin nhận dạng không cần thiết, không suy đoán danh tính/động cơ của khách, và cho phép analyst sửa nhãn.

## Lựa chọn để Deep-Dive

Chọn **Card #2 — Xanh SM xử lý sự cố pin** vì đây là vấn đề real-time, có bottleneck rõ ràng, metric đo được và giá trị của AI nằm ở việc giảm thời gian soạn draft. Rủi ro an toàn được giới hạn bằng các rule cứng, ngưỡng pin 5%, giới hạn 5 km và bước dispatcher duyệt trước khi gửi.

