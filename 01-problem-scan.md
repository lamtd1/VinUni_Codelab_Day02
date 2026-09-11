# Problem scan

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

### 📝 List bài toán của tôi:


| #   | Subsidiary (VinFast/Xanh SM...) | Lens               | Mô tả ngắn bài toán                                                                                                       |
| --- | ------------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| 1   | Xanh SM                         | Pain từ người khác | Điểm đón sai lệch tại chung cư/khu phức hợp/sân bay — tài xế phải gọi điện dò tìm khách, gây thời gian chết và hủy chuyến |
| 2   | VinFast / V-Green               | Lặp lại            | Đối chiếu thủ công sản lượng &amp; hóa đơn sạc điện với hàng nghìn trụ sạc đối tác bên thứ ba mỗi kỳ chốt sổ              |
| 3   | Vinhomes                        | Tốn thời gian      | Phân loại → điều hướng → soạn phản hồi phản ánh cư dân trên App Resident, làm tay ở từng Ban quản lý tòa                  |
| 4   | Vinmec                          | Tốn thời gian      | Bác sĩ soạn tay tóm tắt bệnh án xuất viện + dặn dò; bản giao bệnh nhân toàn thuật ngữ y khoa khó hiểu                     |
| 5   | VinFast                         | AI có thể tốt hơn  | Chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt đời thường của khách ("đi qua gờ kêu cụp cụp") tại call center/xưởng dịch vụ   |


---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách Phase 1 (#1, #3, #4) và hoàn thiện 3 Quick Problem Cards bên dưới.

## Card #1 — Xanh SM: Điểm đón sai lệch tại chung cư/khu phức hợp/sân bay

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Pin GPS đặt xe rơi vào giữa tòa nhà/khu phức hợp/ │
│ sân bay khiến tài xế không tìm được đúng cổng đón khách.    │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau (Actor)? Tài xế (mất thời gian, dễ bị đánh giá  │
│ thấp) và khách hàng (chờ đợi, dễ hủy chuyến).                │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Tài xế nhận cuốc, di chuyển theo pin GPS               │
│   ──> 2. Đến gần điểm đón nhưng không thấy khách             │
│   ──> 3. Gọi điện thoại cho khách hỏi vị trí chính xác       │
│   ──> 4. Khách mô tả bằng lời, tài xế dò tìm/lái vòng quanh  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 5-8 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 & 3 — trích     │
│ xuất địa điểm chi tiết (cổng/sảnh/tầng) từ tin nhắn đặt xe   │
│ hoặc chat với khách, hiển thị ghi chú rõ ràng cho tài xế.    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian tìm khách từ 6 phút ──> dưới 2 phút; giảm    │
│ tỉ lệ hủy chuyến do "không tìm thấy khách" 30%.              │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] LLM  [ ] Rule  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

## Card #2 — Vinhomes: Phân loại & điều hướng phản ánh cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Phản ánh cư dân gửi trên App Resident bị phân loại│
│ và điều hướng thủ công tới đúng bộ phận, làm tay ở từng BQL. │
│ Công ty thành viên: [x] Vinhomes                             │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên Ban quản lý tòa nhà (quá tải,│
│ hàng chục ticket/ngày) và cư dân (chờ phản hồi lâu).         │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                         │
│   1. Cư dân gửi phản ánh (text tự do) qua App                │
│   ──> 2. NV BQL đọc & phân loại hạng mục thủ công            │
│   ──> 3. Điều hướng ticket tới bộ phận kỹ thuật/an ninh       │
│   ──> 4. Soạn phản hồi xác nhận gửi cư dân                   │
│   ──> 5. Bộ phận phụ trách xử lý & cập nhật trạng thái        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 4 (⏱ 4 phút/bước, │
│ tổng 11 phút/lượt cho các bước 1-4)                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4 — tự động     │
│ phân loại hạng mục + mức ưu tiên, soạn draft phản hồi.       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý phản ánh từ 11 phút ──> dưới 3 phút;   │
│ độ chính xác phân loại đạt ≥ 90%.                            │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] LLM  [ ] Rule  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

## Card #3 — Vinmec: Soạn tóm tắt bệnh án xuất viện

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Bác sĩ soạn tay tóm tắt xuất viện + dặn dò; bản    │
│ giao bệnh nhân toàn thuật ngữ y khoa khó hiểu.               │
│ Công ty thành viên: [x] Vinmec                               │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị (quá tải giờ xuất viện) │
│ và bệnh nhân (khó hiểu bản tóm tắt, dễ dặn dò sai).           │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                         │
│   1. Bác sĩ đọc lại toàn bộ hồ sơ điện tử (chẩn đoán, xét    │
│      nghiệm, thuốc đã dùng)                                  │
│   ──> 2. Tự soạn tóm tắt xuất viện bằng thuật ngữ y khoa      │
│   ──> 3. Diễn giải lại cho bệnh nhân dễ hiểu                 │
│   ──> 4. In và giao kèm dặn dò                               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1-2 (⏱ 20-25 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-3 — trích xuất  │
│ & tóm tắt hồ sơ, tạo draft song ngữ (y khoa + dễ hiểu) để    │
│ bác sĩ duyệt lại trước khi giao.                             │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian soạn tóm tắt từ 25 phút ──> dưới 5 phút; 100% │
│ bản tóm tắt được bác sĩ duyệt trước khi giao (HITL bắt buộc).│
│                                                             │
│ Quick Architecture: [ ] No AI  [x] LLM  [ ] Rule  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Quyết định lựa chọn để Deep-Dive

Chọn **Card #2 — Vinhomes: Phân loại & điều hướng phản ánh cư dân** để triển khai Phase 3-5 trong `02-deep-dive-report.md`.

**Lý do lựa chọn:**
* So với Card #1 (Xanh SM), bài toán này không phụ thuộc vào độ chính xác GPS thời gian thực — dữ liệu đầu vào (text phản ánh) sẵn có và ổn định hơn để prototype LLM.
* So với Card #3 (Vinmec), rủi ro khi AI sai thấp hơn nhiều (nhầm hạng mục phản ánh vs. sai thông tin y khoa), nên có thể triển khai GO nhanh hơn mà vẫn giữ HITL ở bước gửi phản hồi.
* Có sẵn dữ liệu lịch sử ticket theo hạng mục tại các BQL để làm baseline đánh giá độ chính xác phân loại.

&nbsp;