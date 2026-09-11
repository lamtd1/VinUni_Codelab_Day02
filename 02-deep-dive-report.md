# Deep Dive Report

> **Bài toán được chọn:** Vinhomes — Phân loại &amp; Điều hướng Phản ánh Cư dân trên App Vinhomes Resident.
> **Nguồn:** Chọn từ Card #2, Phase 2 của [01-problem-scan.md](01-problem-scan.md).

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow Mapping

Xem sơ đồ trực quan tại [04-workflow-diagram.png](04-workflow-diagram.png).


| Bước | Mô tả                                                                                    | Ai thực hiện         | Thời gian            | Ký hiệu       |
| ---- | ---------------------------------------------------------------------------------------- | -------------------- | --------------------: | ------------- |
| 1    | Cư dân gửi phản ánh (text tự do) qua App Vinhomes Resident                               | Cư dân               | 1 phút               |               |
| 2    | Đọc &amp; phân loại thủ công theo hạng mục (điện, nước, thang máy, an ninh, tiếng ồn...) | NV Ban quản lý (BQL) | 4 phút               | 🔴 Bottleneck |
| 3    | Điều hướng ticket tới đúng bộ phận kỹ thuật/an ninh phụ trách                            | NV Ban quản lý       | 2 phút               | 🔄 Handoff    |
| 4    | Soạn phản hồi xác nhận gửi cư dân                                                        | NV Ban quản lý       | 4 phút               | 🔴 Bottleneck |
| 5    | Bộ phận phụ trách xử lý thực địa &amp; cập nhật trạng thái                               | Kỹ thuật / An ninh   | Biến thiên (offline) | 🔄 Handoff    |


**Tổng thời gian xử lý thủ công (Bước 1–4, đo được): 11 phút/lượt.** Bước 5 là xử lý thực địa nên không tính vào SLA phân loại/phản hồi.

**Quan sát:** Bottleneck nằm ở hai bước cần đọc-hiểu ngôn ngữ tự nhiên và soạn thảo văn bản (Bước 2 và Bước 4) — đây chính là loại tác vụ LLM xử lý tốt. Bước 3 (điều hướng) và Bước 5 (xử lý thực địa) là handoff giữa người/bộ phận, ít phù hợp để tự động hóa hoàn toàn vì cần trách nhiệm giải trình rõ ràng theo từng bộ phận.

## 3.2. Problem Statement (6-field) &amp; Metrics


| Field                       | Nội dung chi tiết                                                                                                                                                                                                                                                                                                                                                                             |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Actor / Operator**     | Nhân viên Ban quản lý (BQL) tòa nhà thuộc Vinhomes, người trực tiếp nhận và xử lý phản ánh cư dân trên App Vinhomes Resident mỗi ngày.                                                                                                                                                                                                                                                        |
| **2. Current Workflow**     | Cư dân gửi phản ánh dạng text tự do qua App. NV BQL đọc từng ticket, tự gán hạng mục (điện/nước/thang máy/an ninh/tiếng ồn...) và mức độ ưu tiên, điều hướng ticket tới bộ phận kỹ thuật hoặc an ninh phù hợp, sau đó soạn tin nhắn xác nhận gửi lại cư dân. Toàn bộ 4 bước đầu làm thủ công, mất trung bình 11 phút/lượt.                                                                    |
| **3. Bottleneck**           | Bước 2 (phân loại hạng mục) và Bước 4 (soạn phản hồi xác nhận) — mỗi bước mất ~4 phút vì NV BQL phải đọc hiểu nội dung tự do, tự đối chiếu với danh mục hạng mục nội bộ và tự soạn văn bản trả lời phù hợp giọng điệu dịch vụ.                                                                                                                                                                |
| **4. Business Impact**      | Mỗi tòa nhà nhận trung bình 30-40 phản ánh/ngày. Với 11 phút/lượt, một tòa tiêu tốn ~6-7 giờ nhân lực BQL/ngày chỉ cho việc phân loại và soạn phản hồi. Vào giờ cao điểm (cuối tuần, sau sự cố mất điện/nước diện rộng), tồn đọng ticket khiến thời gian phản hồi kéo dài quá 24 giờ, ảnh hưởng đến điểm hài lòng cư dân (CSAT) và tăng khiếu nại lên cấp cao hơn.                            |
| **5. Success Metric**       | 1. Giảm thời gian xử lý phản ánh (phân loại + điều hướng + soạn phản hồi) từ 11 phút xuống dưới 3 phút/lượt (Efficiency).<br>2. Độ chính xác phân loại hạng mục đạt ≥ 90% so với người (Quality), đo trên tập ticket lịch sử có gán nhãn.                                                                                                                                                     |
| **6. Operational Boundary** | AI được phép: đọc nội dung ticket, đề xuất hạng mục + mức ưu tiên, soạn **draft** phản hồi xác nhận. **TUYỆT ĐỐI KHÔNG được**: tự động gửi phản hồi hoặc đóng ticket mà không có NV BQL phê duyệt (Bắt buộc HITL); tự ý cam kết thời gian xử lý hoặc bồi thường thay cho BQL; xử lý các phản ánh liên quan tranh chấp pháp lý/an toàn tính mạng (phải escalate ngay cho người, không qua AI). |


## 3.3. Future-State Flow &amp; AI Fit

- **AI-Fit Matrix:** [ ] Rule / State-Machine  [x] **LLM Feature**  [ ] Agentic Loop
  - Lý do chọn LLM Feature thay vì Rule: nội dung phản ánh là văn bản tự do tiếng Việt đời thường, biến thiên cách diễn đạt rất lớn — rule/keyword-matching sẽ bỏ sót hoặc phân loại sai nhiều trường hợp.
  - Lý do không cần Agentic Loop: quy trình có cấu trúc cố định (phân loại → điều hướng → soạn draft), không cần AI tự ra quyết định đa bước hay gọi nhiều công cụ ngoài; rủi ro khi tự động hoá toàn trình (gửi thẳng, tự đóng ticket) cao hơn lợi ích.
- **Future-State Flow:**

```markdown
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1        │     │ Bước 2 🔵     │     │ Bước 3 🔵     │     │ Bước 4 🟢     │
│ Cư dân gửi     │     │ AI phân loại  │     │ AI soạn draft │     │ NV BQL review  │
│ phản ánh qua   │ ──→ │ hạng mục +    │ ──→ │ phản hồi xác  │ ──→ │ & click duyệt  │
│ App            │     │ mức ưu tiên   │     │ nhận + route  │     │ gửi cư dân     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                        │
                                                                        ▼
                                                                 ┌──────────────┐
                                                                 │ Bước 5        │
                                                                 │ Bộ phận phụ   │
                                                                 │ trách xử lý   │
                                                                 │ thực địa      │
                                                                 └──────────────┘
                                                                        │
                                                                        ▼
                                                                 ↩️ Fallback:
                                                                 Nếu AI không tự tin
                                                                 phân loại (confidence
                                                                 thấp) hoặc phát hiện
                                                                 từ khóa nhạy cảm
                                                                 (pháp lý/an toàn),
                                                                 route thẳng cho
                                                                 NV BQL xử lý thủ công
                                                                 như quy trình cũ.
```

- 🔵 **AI Step:** Bước 2 (phân loại + gán ưu tiên) và Bước 3 (soạn draft phản hồi + đề xuất bộ phận điều hướng).
- 🟢 **Human Step (HITL):** Bước 4 — NV BQL bắt buộc xem lại và click duyệt trước khi bất kỳ phản hồi nào được gửi cho cư dân.
- ↩️ **Fallback:** Khi AI có độ tự tin thấp hoặc phát hiện nội dung nhạy cảm (tranh chấp pháp lý, đe dọa an toàn), hệ thống tự động chuyển ticket về hàng đợi xử lý thủ công 100% như quy trình hiện tại, không cho AI đề xuất.

---

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:

1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? — Có, lịch sử ticket phản ánh trên App Resident đã được BQL gán nhãn hạng mục trong quá khứ, đủ để làm tập test độ chính xác phân loại.
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? — Có, mọi phản hồi đều qua NV BQL duyệt trước khi gửi; ticket nhạy cảm được fallback về xử lý thủ công.
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? — Có, BQL các tòa đang quá tải vào giờ cao điểm nên chủ động đề xuất công cụ hỗ trợ phân loại nhanh hơn.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:

[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**

> Bài toán hội đủ 3 điều kiện GO: (1) dữ liệu lịch sử sẵn có để đánh giá độ chính xác phân loại trước khi triển khai diện rộng; (2) rủi ro khi AI sai được kiểm soát chặt qua HITL bắt buộc ở bước gửi phản hồi và cơ chế Fallback cho các ticket nhạy cảm — không có kịch bản nào AI tự ý hành động mà không qua người duyệt; (3) giải pháp kỹ thuật đơn giản (LLM Feature phân loại + soạn draft), không cần Agentic Loop phức tạp, nên chi phí phát triển và vận hành thấp trong khi giá trị mang lại rõ ràng (giảm 11 phút xuống dưới 3 phút/lượt, tiết kiệm ~5-6 giờ nhân lực BQL/tòa/ngày). Đề xuất triển khai scope hẹp: thí điểm tại 2-3 tòa nhà có khối lượng ticket cao nhất trong 4 tuần, đo độ chính xác phân loại thực tế và tỉ lệ NV BQL chỉnh sửa draft trước khi mở rộng toàn hệ thống Vinhomes.

