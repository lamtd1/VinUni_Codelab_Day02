# 🏗️ Báo Cáo Phân Tích Sâu (Deep-Dive Report)
## Vin Smart Future — Xanh SM Intelligent Pickup Point Normalization

---

## 📋 Bối Cảnh Chọn Bài Toán

**Bài toán được chọn:** `Card #1 — Xanh SM: Điểm đón lệch, tài xế không tìm thấy khách`

**Lý do chọn Deep-Dive Card #1:**
- Input là **tiếng Việt tự nhiên mơ hồ** → LLM thắng rule tuyệt đối
- Pain point đến từ **tài xế + khách hàng** → dễ bảo vệ trước vận hành
- Impact trực tiếp lên **doanh thu/cuốc và tỉ lệ huỷ chuyến**
- Có thể **prototype trong lab** bằng prompt + mock POI data

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow Mapping

### Quy trình hiện tại: Tài xế tìm khách khi điểm đón GPS lệch

```
┌──────────────────────────────┐
│ Bước 1: Khách đặt chuyến      │
│ Hành động: Khách mở app Xanh  │
│ SM, nhập địa chỉ/vị trí       │
│ Ai: Khách hàng                │
│ ⏱ 1 phút                      │
└──────────────┬───────────────┘
               │
        ↓ 🔄 Handoff (Nền tảng ghi nhận đặt chuyến)
               │
┌──────────────────────────────┐
│ Bước 2: Tài xế nhận cuốc      │
│ Hành động: App tải thông tin  │
│ cuốc + pin điểm đón (GPS)     │
│ Ai: Tài xế (qua app)          │
│ ⏱ 1 phút                      │
└──────────────┬───────────────┘
               │
        ↓ GPS có thể lệch 20-50m
               │
┌──────────────────────────────┐
│ Bước 3: Tài xế tìm khách 🔴  │
│ Hành động: Chạy xe đến vị     │
│ trí GPS, nếu không thấy khách │
│ thì gọi/chat: "anh chị ở     │
│ đâu?" → kiếm trong 50m bán    │
│ kính                          │
│ Ai: Tài xế                    │
│ ⏱ 1-3 phút (thường)           │
└──────────────┬───────────────┘
               │
        ↓ 🔄 Handoff (Gọi điện/chat)
               │
┌──────────────────────────────┐
│ Bước 4: Khách mô tả vị trí 🔴│
│ Hành động: Khách trả lời      │
│ tiếng Việt tự do:             │
│ "cổng sau toà S2",            │
│ "chỗ cây xăng cũ",            │
│ "sảnh B tầng hầm"             │
│ Ai: Khách hàng                │
│ ⏱ 1-2 phút (Lặp lại nếu vẫn   │
│ không tìm được)               │
└──────────────┬───────────────┘
               │
        ↓ Tài xế suy luận, chạy vòng tìm
               │
┌──────────────────────────────┐
│ Bước 5: Gặp nhau hoặc huỷ 🔴 │
│ Hành động:                    │
│  - Nếu tìm được: Khách lên    │
│    xe, bắt đầu chuyến         │
│  - Nếu không tìm (>5 phút):   │
│    Một trong 2 bên huỷ        │
│    chuyến → CSKH xử lý khiếu  │
│    nại                        │
│ Ai: Tài xế + Khách            │
│ ⏱ 2-8 phút                    │
└──────────────────────────────┘

🔴 BOTTLENECKS: Bước 3-5 (tìm khách)
⏱ Thời gian lãng phí trung bình: 2-8 phút/cuốc
📊 Tôn thất: ~30% cuốc có vấn đề tìm khách; 
   5-10% cuốc bị huỷ do "không tìm thấy khách"
```

---

## 3.2. Problem Statement (6-Field) & Metrics

| Field | Chi tiết |
|-------|----------|
| **1. Actor / Operator** | **Tài xế Xanh SM** (chính) + Khách hàng (phụ)<br><br>Khi tài xế không tìm thấy khách đúng giờ, tổng đài CSKH phải xử lý khiếu nại → tăng tải cho team. |
| **2. Current Workflow** | 5 bước thủ công:<br>(1) Khách đặt app → (2) Tài xế nhận pin GPS → (3) Tài xế chạy đến vị trí GPS → (4) Nếu không thấy khách, gọi/chat hỏi → (5) Khách mô tả tiếng Việt tự do ("cổng sau toà", "cây xăng cũ") → Tài xế suy luận & tìm kiếm → Gặp hoặc huỷ. Công cụ: App Xanh SM + Google Maps + cuộc gọi điện. |
| **3. Bottleneck** | **Bước 3-5:** Tài xế chạy đúng pin GPS nhưng GPS lệch trong các khu đô thị lớn, ngõ nhỏ, sảnh TTTM nhiều cửa. Khách phải mô tả vị trí bằng tiếng Việt tự do → tài xế phải gọi lại hỏi nhiều lần, phỏng đoán, chạy vòng tìm. Mất **1-3 phút/cuốc thường; 5-8 phút vào giờ cao điểm hoặc ở khu phức tạp** (VD: Toà nhà có nhiều cửa, sảnh riêng, địa hình lạc đường). |
| **4. Business Impact** | • **Khối lượng:** Xanh SM hiện có ~1000-1500 cuốc/ngày ở Hà Nội; ~30% cuốc gặp vấn đề lệch GPS.<br>• **Thời gian lãng phí:** 1-3 phút × 300-450 cuốc/ngày = 300-1350 phút/ngày (5-22 giờ/ngày) lãng phí tài xế chạy vòng tìm → giảm số cuốc/tài xế → doanh thu mất ~5-10%/ngày trên giờ cao điểm.<br>• **SLA:** Thời gian đón (ETA) bị kéo dài → khách không vừa ý → review xấu, tăng tỉ lệ huỷ chuyến 5-10%.<br>• **CSKH:** Tổng đài phải xử lý ~50-100 cuốc/ngày bị huỷ do "không tìm khách" → tăng tải. |
| **5. Success Metric** | **3 Metric đo:**<br>1. **Hiệu suất tìm khách:** Giảm thời gian tìm từ **2-3 phút** ──> **dưới 45 giây** (khi AI chuẩn hoá được vị trí + tài xế xác nhận).<br>2. **Chất lượng landmark:** ≥**90% landmark** AI trả về khớp đúng **POI thực tế** (verify bằng GPS sau 30 ngày).<br>3. **Tỉ lệ huỷ:** Giảm tỉ lệ huỷ do "không tìm khách" từ **5-10%** ──> **dưới 2%**. |
| **6. Operational Boundary** | **AI được phép:**<br>• Đọc mô tả tiếng Việt của khách ("cổng sau toà S2", "gần cây xăng cũ").<br>• Đối chiếu với danh mục POI cư dân của toà nhà/khu vực từ DB nội bộ (VD: danh sách tên cửa, sảnh, tầng, điểm đón đã thành công trước đó).<br>• Draft lại pin + câu chỉ dẫn ngắn gọn bằng tiếng Việt (VD: *"Đón ở cửa sau toà S2, tầng 1, gần cây xăng cũ"*).<br><br>**AI TUYỆT ĐỐI KHÔNG được:**<br>• Tự động nhắn tin cho tài xế mà không có điều phối viên review (Bắt buộc **HITL** — điều phối viên xem draft, sửa nếu cần, rồi gửi).<br>• Sai lệch >20m so với POI thực tế (nếu LLM trả về >20m thì fallback → điều phối viên xử lý thủ công như trước).<br>• Ghi đè thông tin cũ của khách nếu không có độ tin cay ≥85% từ LLM. |

---

## 3.3. Future-State Flow & AI Fit

### AI Fit Matrix

**Chọn: `[x] LLM Feature` (không cần Agent)**

**Lý do:**
- Quy trình có cấu trúc **cố định & tuyến tính** (5 bước) → không cần agent tự trị.
- Input chính là **tiếng Việt tự nhiên** (mô tả khách) + **dữ liệu cấu trúc** (POI DB) → LLM feature (prompt) đủ.
- Rủi ro thấp khi có **HITL** (điều phối viên duyệt trước khi gửi).

---

### Future-State Workflow

```
┌──────────────────────────────┐
│ Bước 1: Khách đặt & mô tả    │
│ Hành động: Khách mở app      │
│ Xanh SM, nhập địa chỉ +      │
│ mô tả nơi đón (nếu muốn)     │
│ Ai: Khách hàng               │
│ ⏱ 1 phút                     │
└──────────────┬───────────────┘
               │
        ↓ 🔄 Handoff (App lưu)
               │
┌──────────────────────────────┐
│ Bước 2: Tài xế nhận cuốc      │
│ Hành động: App tải thông tin  │
│ cuốc + pin GPS ban đầu        │
│ Ai: Tài xế (qua app)          │
│ ⏱ 1 phút                      │
└──────────────┬───────────────┘
               │
        ↓ Hệ thống gửi thông tin sang LLM
               │
┌──────────────────────────────┐
│ Bước 3: 🔵 LLM chuẩn hoá     │
│ Hành động: LLM nhận mô tả     │
│ khách + POI DB của tòa nhà    │
│ → Trích landmark → Tính lại   │
│ pin GPS dựa trên landmark →   │
│ Draft email/SMS hướng dẫn      │
│ Ai: LLM (Backend)             │
│ ⏱ 3-5 giây                    │
│ Output: JSON chứa pin mới +   │
│ draft SMS                     │
└──────────────┬───────────────┘
               │
        ↓ 🔄 Handoff (Gửi sang duyệt)
               │
┌──────────────────────────────┐
│ Bước 4: 🟢 Điều phối viên     │
│ review & gửi                  │
│ Hành động: Dispatcher xem     │
│ draft (0.5s), nếu đúng click  │
│ "Gửi", nếu sai sửa & gửi     │
│ thủ công                      │
│ Ai: Điều phối viên (HITL)     │
│ ⏱ 5-10 giây                   │
│ Output: SMS gửi đi            │
└──────────────┬───────────────┘
               │
        ↓ SMS tới tài xế + khách
               │
┌──────────────────────────────┐
│ Bước 5: Tài xế tìm & gặp      │
│ Hành động: Tài xế nhận SMS    │
│ + nhấn nút "Đã tìm được       │
│ khách" (xác nhận)             │
│ Ai: Tài xế                    │
│ ⏱ 30-60 giây                  │
│ (GIẢM từ 1-3 phút trước)      │
└──────────────────────────────┘

↩️ FALLBACK:
• Nếu LLM trả về landmark với score <85% → skip LLM, 
  dispatcher xử lý thủ công (như quy trình cũ)
• Nếu tài xế nhận SMS rồi vẫn không tìm → gọi điều phối
  viên (như trước)
```

---

### Thống kê Cải thiện

| Giai đoạn | Thời gian trung bình | Cách thực hiện |
|-----------|----------------------|----------------|
| **Hiện tại (Without AI)** | 2-3 phút/cuốc (1-3 phút tìm + 1 phút gọi hỏi) | Tài xế tìm tay, gọi khách mô tả |
| **Tương lai (With LLM + HITL)** | 45-60 giây/cuốc (~45s LLM + HITL + SMS) | LLM chuẩn hoá, Dispatcher duyệt 1 click |
| **Tiết kiệm** | **60-70%** thời gian/cuốc | Tự động + review, không tìm tay |

---

# 🏁 Phase 5 — EVALUATE

## AI Readiness Checklist

- [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?**
  - ✅ **CÓ:** Danh mục POI của các toà nhà lớn ở Hà Nội đã được ghi nhận trong DB nội bộ của Xanh SM. Tôi có thể lấy mẫu 50-100 cuốc từ log của tuần trước + mô tả khách tương ứng để test prompt.
  - ⚠️ **Lưu ý:** Cần anonymize dữ liệu cá nhân khách (số điện thoại, tên).

- [x] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?**
  - ✅ **CÓ:** Mỗi SMS đều phải qua điều phối viên duyệt trước khi gửi (HITL bắt buộc). Nếu LLM trả về kém chất lượng (score <85%), hệ thống tự động fallback → dispatcher xử lý thủ công như cũ. Không ai bị ảnh hưởng.
  - ⚠️ **Rủi ro nhất:** Nếu dispatcher quá tải, có thể click "Gửi" mà không review → SMS sai landmark gửi đi → tài xế vẫn không tìm được. **Giải pháp:** Giới hạn số SMS gửi/dispatcher/phút; training dispatcher.

- [x] **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?**
  - ✅ **CÓ:** Dispatcher Xanh SM đã phàn nàn về việc tài xế gọi quá nhiều lần hỏi vị trí → Sẵn sàng thử quy trình mới.
  - ⚠️ **Lưu ý:** Cần training dispatcher cách review & sửa draft SMS của LLM (30 phút).

---

## Quyết Định Của Ban Giám Đốc Vin Smart Future

### 🟢 **GO (Bắt đầu xây dựng Prototype)**

**Lựa chọn:** `[x] GO` — Bắt đầu phát triển với scope hẹp.

---

## Justification (Lý Giải Quyết Định)

### ✅ Lý Do GO

1. **Bài toán rõ ràng, có dữ liệu:**
   - Input (mô tả khách tiếng Việt) + Output (landmark chuẩn hoá) đã có lịch sử sẵn.
   - Có danh mục POI DB của Xanh SM để đối chiếu.
   - Có thể prototype trong **3-5 ngày** với 100 sample cuốc.

2. **Impact kinh tế rõ:**
   - Giảm 1.5-2.5 phút/cuốc × 300-450 cuốc/ngày = 450-1125 phút/ngày tiết kiệm.
   - Doanh thu tăng từ việc tài xế có thể nhận cuốc tiếp theo nhanh hơn 30%.
   - Tỉ lệ huỷ giảm → NPS tăng.

3. **Rủi ro thấp với HITL + Fallback:**
   - Mỗi SMS phải qua dispatcher duyệt → không tồn tại "tự động gửi sai".
   - Fallback tự động nếu LLM score thấp.

4. **Alignment với vận hành:**
   - Dispatcher + Tài xế đã sẵn sàng (có phàn nàn).
   - Không cần thay đổi app khách hàng (chỉ backend).

### ⚠️ Điều Kiện GO

1. **Điều kiện kỹ thuật:**
   - LLM phải đạt ≥90% accuracy landmark trên 100 sample test trong vòng 1 tuần.
   - API latency ≤5 giây (nếu >5s, UX xấu).
   - Fallback logic phải hoạt động 100% (không lỗi).

2. **Điều kiện tổ chức:**
   - Cấp phó GĐ Xanh SM phê duyệt allocation **2 dispatcher test** (giờ cao điểm) trong vòng 1 tuần.
   - Dữ liệu POI toàn bộ Hà Nội phải được chuẩn hóa sạch (ước tính 2-3 ngày).

3. **Milestone Phase 4 (Prototype):**
   - **Ngày 1-2:** Fine-tune prompt, test trên 50 sample.
   - **Ngày 3-4:** A/B test với 2 dispatcher (so sánh draft LLM vs thủ công cũ).
   - **Ngày 5:** Nếu accuracy ≥90%, chốt scope MVP & timeline release.

### ❌ Tại sao KHÔNG chọn "NOT YET" hoặc "NO-GO"

- **NOT YET:** Dữ liệu đã có sẵn, baseline đã rõ (2-3 phút hiện tại). Chờ thêm có tính chi phí cao.
- **NO-GO:** LLM + NLP tiếng Việt đã chứng minh được (Gemini/GPT xử lý mô tả địa điểm rất tốt). Rule-based không thể cover được toàn bộ cách người Việt mô tả vị trí (vô hạn).

---

## Kế Hoạch Tiếp Theo (Phase 4 — Prototype)

| Tuần | Hoạt động | Chịu trách nhiệm | KPI |
|------|-----------|-----------------|-----|
| **Tuần 1** | Fine-tune prompt + test 50 sample từ log | AI Engineer | ≥90% accuracy landmark |
| **Tuần 2** | A/B test với 2 dispatcher ở giờ cao điểm | Dispatcher + AI Eng | ≥85% draft được điều phối viên phê duyệt trực tiếp |
| **Tuần 3** | Iterative improvement (feedback từ test) | AI Engineer | Giảm false positive landmark |
| **Tuần 4** | Prepare MVP release spec | Product + Eng | Scope MVP, timeline, SLA |

