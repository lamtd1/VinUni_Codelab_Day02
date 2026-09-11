# 02 — Deep-Dive Report: Xanh SM xử lý sự cố pin

## 1. Problem Statement — 6 fields

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Dispatcher tại Trung tâm Điều vận Xanh SM; tài xế là người báo sự cố và chờ hướng dẫn. |
| **2. Current Workflow** | Tài xế gọi báo hết pin → dispatcher tra vị trí GPS → tra trạm VinFast còn chỗ và loại cổng → soạn tin nhắn chỉ dẫn → gửi qua ứng dụng tài xế hoặc gọi cứu hộ. Quy trình có khoảng 5 bước và mất khoảng 15 phút/lượt. |
| **3. Bottleneck** | Tra cứu trạm phù hợp và soạn hướng dẫn bằng tiếng Việt mất khoảng 10–12 phút. Sai loại cổng, sai khoảng cách hoặc chọn trạm không còn chỗ có thể làm xe tiếp tục cạn pin. |
| **4. Business Impact** | Worked example ước tính khoảng 80 sự cố/ngày tại Hà Nội, tương đương khoảng 20 giờ công/ngày của đội điều vận. Tài xế phải chờ lâu, xe mất thời gian hoạt động và doanh thu có thể bị ảnh hưởng. Các số liệu này cần được xác nhận bằng log vận hành trước khi triển khai rộng. |
| **5. Success Metric** | Giảm tổng thời gian xử lý từ 15 phút xuống dưới 3 phút/lượt. Tỷ lệ hướng dẫn đúng địa điểm và đúng loại cổng đạt ít nhất 98%. Không có trường hợp hệ thống tự gửi tin hoặc chỉ dẫn trạm xa hơn 5 km khi pin dưới 5%. |
| **6. Operational Boundary** | AI được phép đọc dữ liệu GPS/trạm sạc đã được cấp quyền và tạo draft. AI không được tự gửi tin, tự đổi tuyến, tự điều xe hoặc bịa dữ liệu. Pin dưới 5% là critical: không hướng dẫn trạm tiêu chuẩn xa hơn 5 km, phải tạo draft `dispatch_mobile_charger`. Dispatcher bắt buộc duyệt trước mọi hành động. |

## 2. Current-State Workflow

1. Tài xế gọi tổng đài báo sự cố và dispatcher ghi nhận thông tin.
2. Dispatcher tra biển số và vị trí GPS của xe trên bản đồ nội bộ.
3. Dispatcher mở dashboard trạm sạc VinFast, lọc trạm còn trụ trống, khoảng cách và loại cổng phù hợp.
4. Dispatcher tự soạn tin nhắn hướng dẫn bằng tiếng Việt và gửi qua ứng dụng tài xế.
5. Nếu xe đã cạn pin hoặc không thể đến trạm an toàn, dispatcher liên hệ đội cứu hộ.

**Tổng thời gian hiện tại:** khoảng 15 phút/lượt.

**Bottleneck chính:** Bước 3 và 4, vì phải chuyển đổi giữa nhiều màn hình, kiểm tra dữ liệu thủ công và diễn đạt lại hướng dẫn cho từng trường hợp.

## 3. Future-State Flow & AI Fit

### AI Fit

Chọn **LLM Feature**, kết hợp với các rule/state-machine xác định trước. LLM chỉ xử lý tóm tắt và soạn ngôn ngữ; các điều kiện an toàn như ngưỡng pin, khoảng cách, tính tương thích cổng sạc và quyền gửi được kiểm tra bằng rule deterministic.

### Future-State Flow

```text
[1] Nhận cuộc gọi sự cố
        |
        v
[2] Tự động lấy GPS, pin, loại xe và dữ liệu trạm sạc
        |
        v
[3] Rule safety gate: pin < 5% hoặc dữ liệu không đủ?
        | Có                              | Không
        v                                 v
[4A] Draft dispatch_mobile_charger   [4B] Lọc trạm <= 5km, còn chỗ,
     không gửi tự động                    đúng loại cổng
        |                                 |
        +---------------+-----------------+
                        v
              [5] LLM tạo draft JSON/tin nhắn
                        |
                        v
              [6] Dispatcher review và click duyệt
                        |
                +-------+-------+
                |               |
              Duyệt           Từ chối/sửa
                |               |
                v               v
       [7] Gửi/dispatch     [8] Sửa tay hoặc fallback
```

### Structured Output

Mọi output phải bắt đầu bằng `[DRAFT_ONLY] `. Payload phía sau có thể là một trong các dạng sau:

```json
{
  "action": "dispatch_mobile_charger",
  "reason": "Battery level under critical threshold of 5%. Cannot reach station safely."
}
```

Hoặc với tin nhắn thông thường:

```json
{
  "action": "draft_driver_message",
  "message": "Nội dung hướng dẫn dạng nháp",
  "requires_human_approval": true
}
```

### Human-in-the-loop và Fallback

- **HITL:** Dispatcher kiểm tra pin, vị trí, khoảng cách, loại cổng và nội dung draft trước khi gửi.
- **Fallback:** Nếu API/LLM lỗi, dữ liệu thiếu hoặc kết quả không chắc chắn, dispatcher dùng quy trình thủ công hiện tại.
- **Critical battery:** Nếu pin dưới 5%, bỏ qua route đến trạm tiêu chuẩn xa hơn 5 km và chuyển ngay sang draft điều xe sạc di động.
- **Không tự động hóa:** Không cho phép AI tự gửi SMS, tự thay đổi trạng thái chuyến hoặc tự điều xe.

## 4. Đánh giá AI Readiness

| Câu hỏi | Đánh giá | Bằng chứng / hành động tiếp theo |
|---|---|---|
| Có dữ liệu mẫu/logs sạch để test chưa? | **NOT YET** | Cần lấy log sự cố, GPS, pin, trạng thái trạm và loại cổng; ẩn dữ liệu nhận dạng trước khi test. |
| Rủi ro AI sai có được kiểm soát không? | **Có điều kiện** | Có rule pin/khoảng cách, validation dữ liệu, HITL và fallback thủ công. Cần test adversarial trước pilot. |
| Stakeholder sẵn sàng thay đổi quy trình chưa? | **Cần xác nhận** | Dispatcher và tài xế cần được hướng dẫn cách review draft và cách dùng fallback. |

### Quyết định: **GO có giới hạn**

Cho phép bắt đầu prototype/pilot nhỏ ở môi trường kiểm thử, chưa tự động gửi tin và chưa mở rộng production. Trước khi mở rộng cần xác nhận baseline 15 phút/lượt, kiểm tra mục tiêu dưới 3 phút, đánh giá độ chính xác 98% và kiểm thử các tình huống pin dưới 5%.

