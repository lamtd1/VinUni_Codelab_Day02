
# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:

1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
| - | ------------------------------- | ---- | ------------------------ |
| 1 | **Xanh SM** | Pain từ người khác (Stakeholder Pain) | Điểm đón gợi ý bị lệch trong khu đô thị lớn / ngõ nhỏ / sảnh TTTM nhiều cửa. Tài xế và khách phải gọi điện xác nhận lại vị trí, mô tả bằng tiếng Việt tự do ("cổng sau toà S2", "chỗ cây xăng cũ"). Ước tính ~2–3 phút chết/cuốc bị ảnh hưởng, kéo giảm số cuốc/tài xế/ngày và tăng tỉ lệ huỷ chuyến. |
| 2 | **VinFast** | Lặp lại (Repetitive) | Đối chiếu hoá đơn sạc từ các trụ sạc đối tác bên ngoài: kế toán khớp tay hàng chục nghìn dòng log sạc (mã trụ, kWh, thời điểm) với hoá đơn PDF/Excel gửi về mỗi tuần. Ước tính 2–3 nhân sự × 1–2 ngày/tuần, sai lệch tồn đọng làm chậm chu kỳ thanh toán đối tác. |
| 3 | **Vinhomes** | Lặp lại + Tốn thời gian | Phân loại & điều hướng phản ánh cư dân gửi qua App Resident. Nội dung là text tiếng Việt tự do, không theo template ("nhà tôi mất nước từ tối qua", "thang máy toà A kêu lạ"), nhân viên BQL đọc và gán tay về đúng bộ phận (Kỹ thuật / Vệ sinh / An ninh / Dịch vụ). Ước tính 1–2 phút/ticket × hàng trăm ticket/ngày, ~15–20% bị route sai phải chuyển lại làm vỡ SLA phản hồi. |
| 4 | **Vinmec** | Tốn thời gian (Time-consuming) | Soạn thảo tóm tắt xuất viện (Discharge Summary): bác sĩ tổng hợp thủ công bệnh án điện tử, kết quả xét nghiệm, ghi chú điều trị thành bản tóm tắt cho bệnh nhân. Ước tính 20–30 phút/bệnh án, chiếm giờ khám và là nút cổ chai cuối ngày ở khoa nội trú. |
| 5 | **Vinpearl** | AI có thể tốt hơn (AI-upgrade) | Xử lý email Group Booking từ công ty lữ hành: mỗi email dài, phi cấu trúc, nhiều biến thể (số phòng, loại phòng, ngày, suất ăn, trẻ em). Nhân viên Reservation đọc và nhập tay vào PMS. Ước tính 15–25 phút/email, giờ cao điểm tồn email làm chậm phản hồi báo giá → mất đoàn cho đối thủ. |
| 6 | **VinFast** | AI có thể tốt hơn (AI-upgrade) | Chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt của khách khi đặt lịch dịch vụ ("đi qua gờ giảm tốc kêu cụp cụp ở bánh trước"). Hiện CSKH gán nhóm lỗi theo cảm tính → xếp sai kỹ thuật viên/sai khung giờ. Ước tính ~10–15% lịch hẹn phải đổi lại, tăng thời gian xe nằm xưởng. |

> ⚠️ **Lưu ý về số liệu:** Các con số trên là **ước tính giả định để phục vụ scoping** (không phải số liệu nội bộ Vingroup đã được kiểm chứng). Trước khi chốt Problem Statement ở Phase 3, cần verify lại bằng log thật hoặc phỏng vấn vận hành.

**Prompt đã dùng (theo mục TIP):** *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [VinFast / Xanh SM / Vinhomes / Vinmec / Vinpearl]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #___                                     │
│                                                             │
│ Bài toán (1 câu): ________________________________________  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? ______________________________________ │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. ___ ──> 2. ___ ──> 3. ___ ──> 4. ___                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? ___ (⏱ ___ phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? _____________________ │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

**Top 3 đã chọn từ danh sách SCAN:** `#1 (Xanh SM — điểm đón lệch)`, `#2 (VinFast — đối chiếu hoá đơn sạc)`, `#3 (Vinhomes — route phản ánh cư dân)`.

### Card #1 — Xanh SM: Điểm đón lệch, tài xế không tìm thấy khách

```text
┌───────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                         │
│                                                               │
│ Bài toán (1 câu): Điểm đón GPS bị lệch trong khu đô thị /     │
│ ngõ nhỏ, tài xế không tìm thấy khách đúng giờ.                │
│ Công ty thành viên: [x] Xanh SM (GSM)                         │
│                                                               │
│ Ai đang đau (Actor)? Tài xế Xanh SM (chính) + Khách hàng;     │
│ tổng đài CSKH phải xử lý khiếu nại phát sinh.                 │
│                                                               │
│ Workflow thủ công hiện tại (5 bước):                          │
│   1. Khách đặt app, GPS tự gán pin điểm đón                   │
│   ──> 2. Tài xế nhận cuốc, đến gần nhưng không thấy khách     │
│   ──> 3. Tài xế gọi điện / chat: "anh chị đang đứng ở đâu?"   │
│   ──> 4. Khách mô tả tiếng Việt tự do: "cổng sau toà S2",     │
│          "chỗ cây xăng cũ", "sảnh B tầng hầm"                 │
│   ──> 5. Tài xế tự suy luận, chạy vòng tìm khách; quá lâu     │
│          thì một trong hai bên huỷ chuyến                     │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-5                     │
│    (⏱ 2-3 phút/cuốc thường; 5-8 phút ở khu đô thị lớn         │
│     vào giờ cao điểm)                                         │
│                                                               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 4 ──> 5:           │
│ LLM đọc mô tả tiếng Việt của khách + lịch sử điểm đón đã      │
│ thành công tại toạ độ đó, chuẩn hoá thành một landmark cụ     │
│ thể trong danh mục POI nội khu, trả về pin đã chỉnh kèm một   │
│ câu chỉ dẫn ngắn gọn cho tài xế.                              │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                         │
│   1. Giảm thời gian tìm khách: 2-3 phút ──> dưới 45 giây      │
│   2. Giảm tỉ lệ huỷ chuyến do "không tìm thấy khách" 30%      │
│   3. >=90% landmark trả về khớp đúng POI thực tế              │
│                                                               │
│ Quick Architecture: [ ] No AI  [x] Rule (POI lookup)          │
│                     [x] LLM    [ ] Agent                      │
└───────────────────────────────────────────────────────────────┘
```

### Card #2 — VinFast: Đối chiếu hoá đơn sạc của trụ sạc đối tác

```text
┌───────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                         │
│                                                               │
│ Bài toán (1 câu): Đối chiếu hoá đơn sạc điện hằng tuần từ     │
│ hàng trăm trụ sạc đối tác bên ngoài với log sạc nội bộ.       │
│ Công ty thành viên: [x] VinFast (Khối vận hành mạng sạc)      │
│                                                               │
│ Ai đang đau (Actor)? Kế toán công nợ + nhân viên vận hành     │
│ mạng sạc (đối soát trước khi trình thanh toán đối tác).       │
│                                                               │
│ Workflow thủ công hiện tại (5 bước):                          │
│   1. Đối tác gửi hoá đơn PDF/Excel mỗi tuần, mỗi đối tác      │
│      một định dạng khác nhau                                  │
│   ──> 2. Kế toán export log sạc từ hệ thống nội bộ ra Excel   │
│   ──> 3. Khớp tay từng dòng theo mã trụ + mốc thời gian +     │
│          số kWh (VLOOKUP + mắt người đọc)                     │
│   ──> 4. Lập danh sách lệch, soạn email hỏi lại đối tác       │
│   ──> 5. Trình ký duyệt thanh toán                            │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3                       │
│    (⏱ ước tính 2-3 nhân sự x 1-2 ngày/tuần ~ 12h người/tuần)  │
│                                                               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 và Bước 4:       │
│   - Bước 1: LLM đọc hoá đơn đa định dạng ──> JSON chuẩn hoá   │
│   - Bước 4: LLM draft email giải trình chênh lệch             │
│   - Bước 3 (tính toán/khớp số) GIỮ BẰNG RULE-BASED CODE,      │
│     KHÔNG dùng LLM để cộng trừ tiền.                          │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                         │
│   1. >=95% dòng hoá đơn auto-khớp, không cần người can thiệp  │
│   2. Thời gian chốt đối chiếu tuần: 12h ──> dưới 2h           │
│   3. Sai số tiền = 0 (rule tính, LLM chỉ parse)               │
│                                                               │
│ Quick Architecture: [ ] No AI  [x] Rule (matching engine)     │
│                     [x] LLM (parse + draft)  [ ] Agent        │
└───────────────────────────────────────────────────────────────┘
```

### Card #3 — Vinhomes: Phân loại & điều hướng phản ánh cư dân

```text
┌───────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                         │
│                                                               │
│ Bài toán (1 câu): Phân loại và điều hướng phản ánh cư dân     │
│ gửi qua App Vinhomes Resident về đúng bộ phận xử lý.          │
│ Công ty thành viên: [x] Vinhomes (Ban quản lý toà nhà)        │
│                                                               │
│ Ai đang đau (Actor)? Nhân viên Ban quản lý (BQL) trực app;    │
│ cư dân chờ phản hồi; các bộ phận bị chuyển ticket sai.        │
│                                                               │
│ Workflow thủ công hiện tại (5 bước):                          │
│   1. Cư dân gửi phản ánh dạng text tự do + ảnh trên app       │
│   ──> 2. NV BQL đọc, tự gán category và mức ưu tiên           │
│   ──> 3. Chuyển ticket sang bộ phận (Kỹ thuật / Vệ sinh /     │
│          An ninh / Dịch vụ khách hàng)                        │
│   ──> 4. Bộ phận nhận; nếu sai thì chuyển lại (ping-pong)     │
│   ──> 5. NV BQL soạn phản hồi gửi lại cư dân                  │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 và Bước 4             │
│    (⏱ 1-2 phút/ticket để đọc + gán; 15-20% ticket route sai   │
│     phải chuyển lại, kéo phản hồi đầu tiên lên 12h)           │
│                                                               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và Bước 5:       │
│ LLM phân loại category + mức độ khẩn + trích toà/căn hộ từ    │
│ văn bản tiếng Việt, đồng thời draft câu phản hồi cư dân.      │
│ Rule cứng cho từ khoá khẩn cấp (cháy, rò gas, ngập, điện      │
│ giật) ──> route trực tiếp An ninh, bỏ qua LLM.                │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                         │
│   1. >=85% ticket phân loại đúng bộ phận trong dưới 10 giây   │
│   2. Tỉ lệ route sai: 15-20% ──> dưới 5%                      │
│   3. Thời gian phản hồi đầu tiên: 12h ──> dưới 1h             │
│                                                               │
│ Quick Architecture: [ ] No AI  [x] Rule (escalation khẩn cấp) │
│                     [x] LLM (classify + draft)  [ ] Agent     │
└───────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Lý do chọn 3 thẻ này (và loại các thẻ còn lại)

### Tiêu chí sàng lọc tôi dùng

1. **Có dữ liệu để test ngay trong lab** — bài toán nào cần xin dataset nội bộ hoặc dữ liệu nhạy cảm thì loại, vì Phase 4 phải prototype được bằng prompt + vài mẫu tự tạo.
2. **Rủi ro khi AI sai còn kiểm soát được bằng HITL** — sai thì tốn thêm vài phút của nhân viên, không gây hậu quả sức khoẻ hay pháp lý.
3. **Metric đo được bằng con số sẵn có trong hệ thống** (thời gian/lượt, tỉ lệ route sai, tỉ lệ huỷ chuyến) — không phải metric phải đi khảo sát mới có.
4. **Ba thẻ phải khác nhau về kiến trúc** để lúc bảo vệ có cơ sở so sánh Rule vs LLM, thay vì ba thẻ cùng một dạng.

### Vì sao từng thẻ được chọn

| Card | Lý do chọn | Vai trò trong bộ 3 |
| ---- | ---------- | ------------------ |
| **#1 Xanh SM** | Input là **ngôn ngữ tự nhiên tiếng Việt mơ hồ** ("cổng sau toà S2") — đúng chỗ LLM mạnh hơn rule tuyệt đối, vì không thể viết hết luật cho mọi cách người Việt mô tả vị trí. Pain đến từ chính tài xế và khách nên rất dễ bảo vệ trước vận hành. | Ứng viên mạnh nhất cho Deep-Dive: real-time, NLP-heavy, impact trực tiếp lên doanh thu/cuốc. |
| **#2 VinFast** | Là **case đối chứng cố tình**: khối lượng lặp lại lớn, tổn thất rõ bằng giờ người, nhưng phần lõi (khớp số tiền) **phải là rule-based code**, LLM chỉ được phép parse hoá đơn đa định dạng và draft email. Có thẻ này thì phần *"vì sao không dùng AI cho bước X"* trong Phase 5 có bằng chứng thật. | Cột mốc so sánh: chứng minh tôi không nhét AI vào mọi chỗ. |
| **#3 Vinhomes** | Bài toán **classification + routing** kinh điển, có ground-truth sẵn: lịch sử ticket đã được gán nhãn và đã biết ticket nào từng bị chuyển sai → đo accuracy được ngay, không cần khảo sát. Rủi ro sai thấp (route sai thì chuyển lại). | Dự phòng an toàn nhất nếu nhóm muốn scope hẹp, dễ đạt metric. |

### Vì sao loại 3 thẻ còn lại

* **#4 Vinmec (tóm tắt xuất viện)** — Impact cao nhất về giờ bác sĩ, nhưng đây là **văn bản y tế**: một câu hallucinate về thuốc/liều là rủi ro an toàn bệnh nhân và pháp lý, không phải rủi ro vận hành. Muốn làm tử tế thì Operational Boundary phải rất ngặt (bác sĩ ký từng bản) và cần dữ liệu bệnh án thật — vượt phạm vi một lab 3 tiếng.
* **#5 Vinpearl (email Group Booking)** — Về bản chất rất giống #2 (đọc văn bản phi cấu trúc → trích field → nhập hệ thống). Giữ cả hai là **trùng dạng bài**, nên tôi giữ #2 vì tổn thất định lượng rõ hơn và có sẵn log nội bộ để đối chiếu.
* **#6 VinFast (chẩn đoán lỗi xe từ mô tả khách)** — Ý tưởng hay và cùng dạng NLP tiếng Việt như #1, nhưng để gán được mã lỗi kỹ thuật thì cần **taxonomy lỗi + dữ liệu lịch sử sửa chữa** mà tôi không truy cập được trong lab. Ngoài ra nó chạm vào an toàn xe, cần kỹ thuật viên xác nhận. Xếp vào backlog Phase 2 sau.

> **Đề xuất cho nhóm ở Phase 3:** chọn **Card #1 (Xanh SM)** để Deep-Dive. Nếu nhóm muốn chắc điểm hơn về khả năng đạt metric thì lấy **Card #3 (Vinhomes)** — cùng dạng LLM feature nhưng rủi ro thấp hơn và có ground-truth sẵn.


> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*
