# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 |VinFast (Sales & Warranty Ops) | Time-consuming|Chuyên viên phải đọc thủ công từng hồ sơ claim bảo hành (mô tả lỗi, ảnh, lịch sử xe) để quyết định duyệt/từ chối |
| 2 |VinFast (Service Center) | AI-upgrade|Kỹ thuật viên chẩn đoán lỗi pin/motor dựa trên kinh nghiệm cá nhân thay vì dữ liệu cảm biến, dẫn đến thời gian xe nằm xưởng kéo dài |
| 3 |Xanh SM | Stakeholder Pain| Tài xế tự quyết định di chuyển đến khu vực nào khi rảnh (idle) mà không có gợi ý dựa trên dự báo cầu, gây lệch cung-cầu cục bộ|
| 4 |V-Green (Charging) |Repetitive |Nhân viên vận hành phải rà soát thủ công log trạm sạc để phát hiện trụ lỗi/offline thay vì có cảnh báo tự động real-time |
| 5 | VinFast (CSKH)|AI-upgrade |Tổng đài/chat hỗ trợ khách hàng trả lời các câu hỏi lặp lại (lịch bảo dưỡng, lỗi cảnh báo, chính sách bảo hành) bằng kịch bản cứng, chậm và không cá nhân hóa |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

Top 3 được chọn: **#1 (VinFast Warranty Claim)**, **#4 (V-Green Trạm sạc lỗi)**, **#5 (VinFast CSKH)**.

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Chuyên viên bảo hành phải đọc thủ công từng hồ sơ  │
│ claim (mô tả lỗi, ảnh, lịch sử xe) để quyết định duyệt/từ chối│
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Chuyên viên xử lý claim bảo hành (Warranty Claim Specialist) │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Đại lý gửi hồ sơ claim ──> 2. Chuyên viên đọc mô tả lỗi │
│   ──> 3. Đối chiếu ảnh/video với lịch sử bảo dưỡng xe        │
│   ──> 4. Tra chính sách bảo hành theo dòng xe/thời hạn       │
│   ──> 5. Ra quyết định duyệt/từ chối + ghi chú lý do         │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 18 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4              │
│ (Tóm tắt hồ sơ, đối chiếu chính sách, draft khuyến nghị)    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   "Giảm thời gian xử lý 1 claim từ 25 min ──> dưới 8 min"   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Nhân viên vận hành phải rà soát thủ công log trạm  │
│ sạc để phát hiện trụ lỗi/offline thay vì cảnh báo tự động   │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [x] Khác (V-Green)________ │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên vận hành trạm sạc V-Green     │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Đăng nhập dashboard log trụ sạc ──> 2. Lọc thủ công    │
│   theo từng khu vực ──> 3. Đối chiếu trạng thái bất thường  │
│   ──> 4. Gọi điện xác minh & báo kỹ thuật viên đến sửa       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 30 phút/ca trực)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (Tự động phát hiện anomaly trong log & cảnh báo real-time)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   "Giảm thời gian phát hiện trụ lỗi từ 30 min ──> dưới 5 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Tổng đài/chat CSKH VinFast trả lời câu hỏi lặp lại │
│ (lịch bảo dưỡng, lỗi cảnh báo, chính sách) bằng kịch bản cứng│
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Khách hàng VinFast & nhân viên tổng đài │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khách hàng gửi câu hỏi ──> 2. Nhân viên tra kịch bản   │
│   có sẵn ──> 3. Tìm thông tin xe/lịch sử trong hệ thống nội │
│   bộ ──> 4. Soạn & gửi phản hồi                              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 6 phút/lượt)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (Chatbot trả lời câu hỏi thường gặp, tra cứu tự động)       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   "Giảm thời gian phản hồi trung bình từ 6 min ──> dưới 1 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---