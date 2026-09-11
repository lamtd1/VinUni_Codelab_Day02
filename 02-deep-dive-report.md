# 🏗️ Báo Cáo Phân Tích Sâu (Deep-Dive Report) — Vin Smart Future

> **Dự án lựa chọn:** Card #1 — Vinhomes: Phân loại, điều hướng ticket khiếu nại và gợi ý phản hồi cá nhân hóa cho cư dân trên App Vinhomes Resident.  
> **Đơn vị thành viên:** Vinhomes (Khối Quản lý & Vận hành Khu đô thị).  
> **Người thực hiện:** Nguyễn Xuân Khuê — Ban Công nghệ Vin Smart Future.

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)

## 3.1. Current-State Workflow Mapping

Quy trình xử lý phản hồi/khiếu nại của cư dân trên hệ thống CRM Vinhomes hiện tại:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận ticket  │     │ Đọc hiểu &   │     │ Gán nhãn &   │     │ Soạn phản hồi│
│ qua App      │ ──→ │ phân tích ý  │ ──→ │ Route tổ đội │ ──→ │ xác nhận     │
│              │     │              │     │              │     │              │
│ Ai: Hệ thống │     │ Ai: CSKH     │     │ Ai: CSKH     │     │ Ai: CSKH     │
│ ⏱ 0 phút     │     │ ⏱ 2 phút 🔴  │     │ ⏱ 1 phút 🔴  │     │ ⏱ 2 phút 🔴  │
│ In: App      │     │ In: Text/Ảnh │     │ In: Ý định   │     │ In: Lịch sử  │
│ Out: Queue   │     │ Out: Vấn đề  │     │ Out: Phiếu   │     │ Out: Văn bản │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Gửi tin &    │
                                                               │ Theo dõi     │
                                                               │ Ai: CSKH     │
                                                               │ ⏱ <1 phút    │
                                                               └──────────────┘
```

* 🔴 **Bottleneck:** Bước 2, 3, 4 (Đọc hiểu, Gán nhãn, Soạn phản hồi) gây tắc nghẽn nghiêm trọng.
* ⏱ **Tổng thời gian xử lý thủ công:** ~5–6 phút/lượt.

---

## 3.2. Problem Statement (6-field) & Metrics

Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
| :--- | :--- |
| **1. Actor / Operator** | Nhân viên Chăm sóc khách hàng (CSKH) hoặc nhân sự thuộc Ban Quản lý (BQL) khu đô thị trực trên hệ thống CRM của Vinhomes. |
| **2. Current Workflow** | (1) Cư dân tạo ticket (text/hình ảnh) trên App Vinhomes Resident ──> (2) Ticket đổ về hàng đợi (queue) chung trên CRM ──> (3) Nhân viên CSKH mở từng ticket, đọc và phân tích ngữ cảnh ──> (4) Gán nhãn thủ công (Kỹ thuật điện/nước, Vệ sinh, An ninh...) và chuyển tiếp (route) cho tổ đội tương ứng ──> (5) Copy/paste hoặc gõ tay phản hồi xác nhận cho cư dân theo kịch bản có sẵn. |
| **3. Bottleneck** | Chậm và dễ nghẽn nhất ở khâu đọc hiểu ngữ cảnh và gán nhãn (Bước 3 & 4). Cư dân thường viết không chuẩn format, dùng từ lóng, hoặc gộp nhiều vấn đề vào một ticket (VD: *"Hành lang tầng 10 mùi rác nồng nặc, tiện thể xem lại đèn thang máy bị nhấp nháy từ sáng"*). Việc xử lý đa ý định (multi-intent) khiến CSKH bối rối, dễ route nhầm hoặc sót việc, tốn 3–5 phút/ticket. |
| **4. Business Impact** | Khi có sự cố diện rộng (ví dụ: cúp điện tòa nhà), lượng ticket tăng đột biến (Spike) khiến CSKH bị quá tải. Hậu quả là vi phạm cam kết SLA (thời gian phản hồi lần đầu - First Response Time bị kéo dài từ 15 phút lên vài tiếng), cư dân nhận lại những phản hồi rập khuôn gây ức chế, làm sụt giảm nghiêm trọng điểm hài lòng khách hàng (CSAT/NPS). |
| **5. Success Metric** | • **AI Routing Accuracy:** > 85% ticket được gán nhãn và route đúng phòng ban tự động dưới 5 giây.<br>• **First Response Time (FRT):** Giảm thời gian CSKH xử lý và phản hồi vé từ trung bình 5 phút ──> dưới 1 phút/lượt (nhờ AI draft sẵn câu trả lời). |
| **6. Operational Boundary** | • **Được phép:** AI tự động đọc hiểu, trích xuất thực thể (số căn hộ, loại sự cố), gán tag, route ticket, và sinh ra (draft) câu trả lời nháp cá nhân hóa theo đúng tone-of-voice của Vinhomes.<br>• **TUYỆT ĐỐI KHÔNG được làm:** Không tự động gửi phản hồi trực tiếp (auto-reply) với các ticket liên quan đến khiếu nại gay gắt, pháp lý, tài chính (phí dịch vụ, sổ hồng, bồi thường). Không tự động đổi trạng thái ticket thành "Resolved" (Đã giải quyết).<br>• **Điểm cần duyệt (Human-in-the-loop):** Nhân viên CSKH bắt buộc phải đọc lại bản draft của AI và bấm "Approve/Gửi" để phản hồi tới cư dân. |

---

## 3.3. Future-State Flow & AI Fit

* **Xác định mức AI Fit (AI-Fit Matrix):**  
  `[ ] Rule / State-Machine` &nbsp;|&nbsp; `[x] LLM Feature` &nbsp;|&nbsp; `[ ] Agentic Loop`  
  *(Lý do: Quy trình xử lý ticket có cấu trúc phân luồng cố định, tác vụ cốt lõi là đọc hiểu ngôn ngữ tự nhiên phi cấu trúc và trích xuất thực thể. Không cần Agent tự trị vì rủi ro phát ngôn sai lệch cần kiểm soát chặt chẽ qua Human-in-the-loop).*

* **Sơ đồ luồng tương lai (Future-State Flow):**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận ticket  │     │ 🔵 AI đọc,   │     │ 🔵 AI route &│     │ 🟢 CSKH đọc  │
│ qua App      │ ──→ │ phân loại &  │ ──→ │ Draft phản   │ ──→ │ lại draft,   │
│              │     │ lấy Entity   │     │ hồi nháp     │     │ bấm "Gửi"    │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                │
                                ▼
                        ↩️ Fallback:
                        Nhận diện từ khóa nhạy cảm
                        (kiện, bồi thường, báo chí)
                        hoặc Confidence < 70% 
                        ──> Bỏ qua AI, chuyển thẳng 
                        vào hàng đợi ưu tiên cho 
                        Quản lý CSKH tự xử lý.
```

* 🔵 **AI Step:** Tác vụ LLM xử lý đọc hiểu, phân loại nhãn, trích xuất thực thể và sinh draft phản hồi.
* 🟢 **Human Step (HITL):** Nhân viên CSKH kiểm tra lại bản draft và bấm "Phê duyệt/Gửi".
* ↩️ **Fallback:** Kế hoạch dự phòng chuyển thẳng Quản lý khi gặp từ khóa nhạy cảm hoặc độ tin cậy thấp.

---

# 🏁 Phase 5 — EVALUATE (Nhóm)

### AI Readiness Checklist:
- [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
- [x] Dữ liệu hiện tại đã đủ tốt/sạch để làm Ground Truth hoặc Prompt Context?
- [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

---

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
- [x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
- [ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
- [ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

---

### Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):

> Dự án **"Phân loại, điều hướng ticket và gợi ý phản hồi trên App Vinhomes"** được đánh giá **GO** để tiến hành phát triển bản Prototype (MVP) dựa trên 3 trụ cột sau:
> 
> **1. Bằng chứng khả thi về Kỹ thuật (Technical Feasibility):**  
> Kết quả thử nghiệm ranh giới (Boundary Test) ở Phase 4 chứng minh LLM hoàn toàn có khả năng đọc hiểu ngôn ngữ phi cấu trúc, tiếng lóng và văn phong phàn nàn của cư dân để bóc tách chính xác Intent và Entity. Cấu trúc ép đầu ra dạng JSON hoạt động ổn định, dễ dàng tích hợp qua API vào hệ thống CRM hiện tại mà không cần đập đi xây lại hạ tầng.
> 
> **2. Kiểm soát Rủi ro tuyệt đối (Risk Mitigation):**  
> Luồng vận hành (Future-State Flow) đã khóa chặt rủi ro bằng hai lớp bảo vệ:
> - **Fallback Route:** Tự động bắt keyword nhạy cảm (báo chí, bồi thường, pháp lý) để chuyển ngay cho Quản lý xử lý thủ công.
> - **Human-in-the-Loop (HITL):** CSKH bắt buộc phải duyệt (Approve) bản draft của AI. AI không có quyền tự động gửi tin (auto-reply), đưa rủi ro phát ngôn sai lệch về 0%.
> 
> **3. Hiệu quả chi phí và Business ROI:**  
> Giải quyết triệt để nút thắt (bottleneck) gây chậm trễ SLA. Việc giảm thời gian xử lý thủ công từ 5–6 phút/ticket xuống dưới 1 phút giúp các Ban Quản lý Vinhomes tiết kiệm hàng nghìn giờ công mỗi tháng, tránh tình trạng "vỡ trận" tổng đài khi có sự cố diện rộng. Chi phí gọi API LLM (dự kiến vài trăm đồng/ticket) là vô cùng nhỏ so với chi phí tuyển thêm nhân sự thời vụ và giá trị của việc bảo vệ điểm hài lòng khách hàng (CSAT/NPS).

---