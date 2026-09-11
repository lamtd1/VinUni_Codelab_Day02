# 🔍 Quét Bài Toán & Đánh Giá Nhanh (Problem Scan & Quick-Assess) — Lab 02

> **Học viên thực hiện:** Nguyễn Xuân Khuê  
> **Vị trí:** AI Engineer — Ban Công nghệ Vin Smart Future (Vingroup)  
> **Nhiệm vụ:** Phase 1 (SCAN — Quét 5 bài toán thực tế qua 4 Lenses) & Phase 2 (QUICK-ASSESS — 3 Quick Problem Cards)  
> **Dự án chọn Deep-Dive:** Card #1 — Phân loại & điều hướng ticket khiếu nại cư dân Vinhomes

---

## 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Sử dụng **4 Lenses** (Lặp lại, Tốn thời gian, AI-upgrade, Pain từ người khác) để quét qua các luồng vận hành thực tế tại các công ty thành viên Vingroup.

### 📝 Bảng danh sách bài toán vận hành:

| # | Đơn vị (Subsidiary) | Lăng kính (Lens) | Mô tả ngắn bài toán & điểm nghẽn thực tế |
| :-: | :--- | :--- | :--- |
| **1** | **Xanh SM** | Lặp lại | So khớp và phân bổ lại cuốc xe khi khách hàng yêu cầu thay đổi điểm đến giữa chừng. |
| **2** | **Vinhomes** | AI-upgrade | Hệ thống phân loại và route tự động các phản hồi/khiếu nại của cư dân trên App Vinhomes Resident (CSKH phản hồi rập khuôn, mất 12 tiếng). |
| **3** | **VinFast** | AI-upgrade | Trợ lý ảo trên xe thỉnh thoảng phản hồi theo kịch bản cứng, chưa hiểu sâu ngữ cảnh hội thoại liên tục hoặc từ lóng vùng miền của tài xế. |
| **4** | **Vinmec** | Tốn thời gian | Bộ phận CSKH gọi điện thoại thủ công nhắc lịch hẹn khám/nội soi cho hàng trăm bệnh nhân mỗi ngày và cập nhật trạng thái hủy lịch. |
| **5** | **VinFast** | Pain từ người khác | Kỹ thuật viên xưởng dịch vụ phàn nàn vì phải tra cứu thủ công hàng nghìn trang tài liệu sửa chữa (Service Manual) để tìm hướng dẫn xử lý cho một mã lỗi DTC phức tạp. |

---

## 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Lựa chọn **top 3 bài toán** tiềm năng nhất từ Phase 1 và hoàn thiện **3 Quick Problem Cards**.

### 🃏 Quick Problem Card #1: Phân loại & điều hướng ticket khiếu nại cư dân (Vinhomes)
> ⭐ **Dự án được chọn cho Phase 3 — Deep-Dive Report & Prototyping**

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Phân loại, điều hướng ticket khiếu nại và │
│ gợi ý phản hồi cá nhân hóa cho cư dân trên App Vinhomes.    │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Cư dân (chờ lâu) & CSKH (quá tải ticket)│
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận ticket ──> 2. Đọc hiểu ──> 3. Route phòng ban ──> │
│   4. Soạn phản hồi mẫu                                      │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 3-5 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Phân loại Intent ở    │
│ bước 2, tự động route ở bước 3, gen template trả lời bước 4.│
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian route  │
│ từ 12h ──> under 5 min, Accuracy/F1-score phân loại > 85%.  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

### 🃏 Quick Problem Card #2: Trợ lý AI tra cứu sổ tay mã lỗi DTC cho KTV (VinFast)

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Trợ lý AI hỗ trợ KTV tra cứu nhanh hướng  │
│ dẫn xử lý mã lỗi (DTC) từ hàng nghìn trang Service Manual.  │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Kỹ thuật viên (KTV) xưởng dịch vụ.     │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Quét mã DTC ──> 2. Mở PDF Manual ──> 3. Ctrl+F tìm mã  │
│   ──> 4. Đọc hiểu & đối chiếu sơ đồ để tháo lắp.            │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 15-20 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4 (Trích xuất│
│ chính xác đoạn text/sơ đồ cách xử lý theo truy vấn mã lỗi). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian tìm    │
│ kiếm tài liệu từ 20 min ──> under 1 min, tăng năng suất/ca. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

### 🃏 Quick Problem Card #3: Voicebot tự động nhắc lịch khám bệnh & cập nhật HIS (Vinmec)

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Voicebot tự động gọi điện nhắc lịch hẹn   │
│ khám bệnh và cập nhật trạng thái (hủy/đổi) lên hệ thống HIS.│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Lễ tân/CSKH (ngốn thời gian làm tay chân)│
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Lọc DS khám ──> 2. Gọi từng số ──> 3. Ghi nhận ý định  │
│   (xác nhận/hủy) ──> 4. Cập nhật thủ công lên hệ thống.     │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 3-5 phút/cuộc)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tự động gọi (bước 2), │
│ nhận diện ý định bệnh nhân (bước 3) & trigger API (bước 4). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Tiết kiệm 100% thời   │
│ gian gọi thủ công, giảm tỷ lệ No-show xuống dưới 5%.        │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```