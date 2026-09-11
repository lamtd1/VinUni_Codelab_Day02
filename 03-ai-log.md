# 📔 AI Log — Nhật Ký Tương Tác AI Trong Quá Trình Scoping

**Người viết:** Tôi (AI Engineer tại Vin Smart Future)  
**Ngày:** 11 tháng 9, 2026  
**Bài toán chọn:** Xanh SM — Điểm đón lệch, tài xế không tìm thấy khách  

---

## 🤖 Phần 1: AI Giúp Gì Trong Phase 1-2 (Brainstorm & Sàng Lọc)

### Prompt Đầu Tiên — Brainstorm Bài Toán

**Prompt tôi dùng:**

```
Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành 
cụ thể có thể tối ưu bằng AI cho mảng Xanh SM (dịch vụ taxi điện). Hãy gợi ý cho tôi 5 quy trình 
nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất.
```

**AI trả lời (ChatGPT):**

Kết quả: AI đề xuất 5 bài toán:
1. ✅ Điều vận thông minh (Smart Dispatching) — tối ưu hóa điểm đón taxi
2. ✅ Phân tích lý do hủy chuyến từ ghi âm cuộc gọi
3. ✅ Route lại chuyến khi khách yêu cầu thay đổi
4. ✅ Xử lý phản hồi khẩn cấp từ tài xế
5. ✅ Phân loại feedback khách hàng

**Đánh giá:** AI trả lời hay, nhưng **số liệu mang tính chất ước tính** (vì AI không truy cập được dữ liệu nội bộ Xanh SM). Tôi đã flag rõ ràng ở Phase 1 rằng cần verify lại bằng log thật.

---

### Prompt Thứ 2 — So Sánh 3 Thẻ Để Chọn Top 3

**Prompt tôi dùng:**

```
Đây là 6 bài toán tôi brainstorm cho Xanh SM:
[Dán danh sách 6 bài toán]

Tôi muốn chọn 3 để Deep-Dive trong lab 3 tiếng. Tiêu chí:
1. Có dữ liệu mẫu để test ngay (không cần xin dataset nhạy cảm nội bộ)
2. Rủi ro khi AI sai không gây hậu quả sức khoẻ/pháp lý
3. Metric đo được bằng con số sẵn trong hệ thống
4. Ba thẻ phải khác nhau về kiến trúc (Rule vs LLM vs Agent)

Hãy giải thích tại sao từng thẻ được chọn hoặc loại.
```

**AI trả lời:**

✅ **Đúng:** AI đưa ra reasoning logic, phân tích từng tiêu chí.  
⚠️ **Sai sót:** AI đề xuất luôn chọn bài toán **"Phân loại feedback"** (vì "ground-truth có sẵn") mà không xem xét bài toán **"Điểm đón lệch"** có impact cao hơn.

**Tôi sửa prompt:**

```
Thay vì chọn cơ cơ nhất, hãy xếp hạng 6 bài toán theo:
- Impact kinh tế (doanh thu, giờ người lãng phí)
- Độ phức tạp NLP (để chứng minh LLM mạnh hơn rule)
- Rủi ro operational

Mục tiêu: Tôi muốn bài toán #1 để showcase AI, bài toán #2 để làm "case đối chứng" 
(chứng minh không phải lúc nào AI cũng tốt), bài toán #3 để fallback nếu #1 khó.
```

**AI trả lời lần 2:**

✅ **Tốt hơn:** AI xếp hạng lại:
1. **Điểm đón lệch** (impact cao + NLP heavy + tài xế/khách phàn nàn)
2. **Đối chiếu hoá đơn sạc** (khối lượng lặp lại lớn + proof AI không lúc nào phù hợp)
3. **Phân loại phản ánh cư dân** (safe, có ground-truth)

**Kết luận:** Prompt lần 2 tốt hơn vì tôi **cung cấp rõ mục đích** thay vì chỉ nói "hãy chọn".

---

## 🤖 Phần 2: AI Giúp Gì Trong Phase 3 (Deep-Dive)

### Prompt 3 — Thiết Kế Current-State Workflow

**Prompt tôi dùng:**

```
Bài toán: Điểm đón GPS bị lệch ở Hà Nội → tài xế không tìm thấy khách.

Bước hiện tại (thủ công):
1. Khách đặt app, GPS gán pin
2. Tài xế nhận cuốc
3. Tài xế chạy đến pin GPS
4. Không thấy khách → gọi hỏi
5. Khách mô tả tiếng Việt tự do ("cổng sau toà S2", "cây xăng cũ")
6. Tài xế tìm kiếm + gặp hoặc huỷ

Bạn hãy vẽ lại workflow này dưới dạng ASCII diagram + đánh dấu:
- 🔴 Bottleneck (bước tốn thời gian nhất)
- 🔄 Handoff (điểm chuyển giao thông tin)
- Thời gian mỗi bước

Cung cấp con số ước tính tổn thất (khối lượng × thời gian × tác động).
```

**AI trả lời:**

✅ **Vẽ diagram ASCII rất tốt**, ghi rõ các ký hiệu, thời gian.  
⚠️ **Thiếu:** Số liệu cụ thể (bao nhiêu cuốc/ngày bị ảnh hưởng).

**Tôi sửa prompt:**

```
Thêm vào: Giả sử Xanh SM có ~1000-1500 cuốc/ngày ở Hà Nội.
Nếu 30% bị lệch GPS, đó là 300-450 cuốc/ngày.
Nếu mỗi cuốc tốn 2-3 phút tìm, tổng lãng phí = ?
Hãy tính toán tổn thất theo:
- Giờ người lãng phí (phút × cuốc / 60)
- Doanh thu mất (nếu tài xế không nhận cuốc tiếp theo vì bận tìm)
- Tỉ lệ huỷ chuyến tăng
```

**AI trả lời lần 2:**

✅ **Tính toán chính xác:** 300-450 cuốc × 2.5 phút = 750-1125 phút/ngày = 12-19 giờ lãng phí/ngày.  
✅ **Ước tính doanh thu mất:** Tài xế bình quân nhận 10-15 cuốc/ngày → Nếu mất 2-3 cuốc do bận tìm → mất 15-20% doanh thu giờ cao điểm.

**Kết luận:** Cung cấp **context & số liệu gốc** giúp AI tính toán chính xác hơn.

---

### Prompt 4 — Thiết Kế Future-State + LLM Fit

**Prompt tôi dùng:**

```
Giả sử tôi dùng LLM để chuẩn hoá mô tả khách ("cổng sau toà S2" → landmark POI cụ thể).

Future-state workflow:
1. Khách gửi mô tả tiếng Việt
2. LLM nhận mô tả + POI DB → Trích landmark → Tính pin GPS mới
3. Dispatcher review draft SMS
4. SMS gửi tài xế
5. Tài xế tìm (nhanh hơn vì landmark rõ)

Bạn hãy:
a) Xác định bước nào là AI Step (🔵), bước nào là Human Step (🟢)
b) Thiết kế Fallback: khi nào thì LLM output không tốt? Làm gì?
c) Tính thời gian mỗi bước và so sánh với workflow cũ

Cảnh báo: "KHÔNG dùng AI tự động gửi SMS mà không có dispatcher review!" 
Lý do gì?
```

**AI trả lời:**

✅ **Phân tích đúng bước AI vs Human** — LLM ở bước 2, Dispatcher ở bước 3.  
✅ **Fallback logic rõ:** Nếu LLM score <85%, skip LLM → dispatcher xử lý thủ công.  
✅ **Giải thích HITL:** "Nếu không review, SMS có thể sai landmark → tài xế vẫn không tìm được → huỷ chuyến → bad experience."

⚠️ **Thiếu:** Không đề cập đến **khả năng hallucinate của LLM** — LLM có thể tạo ra "landmark giả" nếu dataset POI không đầy đủ.

**Tôi sửa prompt:**

```
Thêm vào: Giả sử POI DB của toà nhà A chỉ có 10 landmark được ghi nhận.
Khách mô tả: "Gần cây vú sữa bên phải".

LLM có thể:
a) Match đúng với POI "Cây xanh khu vực sảnh B" (đúng)
b) Hallucinate: Tạo ra POI giả "Cây vú sữa Sảnh D" không tồn tại (SAI)
c) Return score thấp: "Không tìm được POI phù hợp" (SAFE)

Hãy thiết kế validation:
- Nếu LLM match được POI từ DB → score cao (≥85%) → draft SMS
- Nếu LLM hallucinate (tạo POI mới không trong DB) → score thấp → fallback
- Nếu không match → score = 0 → fallback

Làm sao kiểm tra LLM có hallucinate hay không?
```

**AI trả lời lần 2:**

✅ **Rất tốt:** Đề xuất validation bằng:
- Vector similarity: POI trả về phải có cosine similarity ≥0.9 với POI trong DB.
- Post-check: Sau 30 ngày, verify thực tế tài xế có tìm thấy không (feedback từ app).
- Rule cứng: Landmark trả về phải nằm trong bán kính 50m của POI trong DB.

**Kết luận:** Prompt càng cụ thể (kèm use case hallucinate) → AI trả lời càng ngăn chặn rủi ro.

---

## 🚨 Phần 3: AI Sai Sót & Cách Tôi Sửa

### Sai Sót #1: AI Đề Xuất Agenttic Loop

**Prompt ban đầu (vô tình):**

```
Bạn hãy thiết kế kiến trúc AI cho bài toán này. 
Nên dùng Rule, LLM Feature, hay Agentic Loop?
```

**AI trả lời (Gemini):**

```
Nên dùng Agentic Loop:
- Agent 1: Xử lý mô tả khách
- Agent 2: Tra cứu POI DB
- Agent 3: Tính lại GPS
- Agent 4: Soạn SMS

Lợi ích: Tự trị, linh hoạt, mở rộng được.
```

❌ **Đó là SAI.** Lý do:
- Quy trình chỉ có 5 bước, **cố định & tuyến tính** → Agentic loop **gây overhead**.
- Thêm độ phức tạp → khó debug khi sai.
- Rủi ro cao: Agent có thể quyết định tự động gửi SMS mà không review → nguy hiểm.

**Tôi sửa prompt:**

```
Giả sử quy trình CHỈ có 2 bước cốt lõi:
1. Nhận input → LLM chuẩn hoá → Output landmark
2. Dispatcher review → Gửi SMS

Không có "tự trị" - luôn luôn có người quyết định gửi SMS hay không.

Theo nguyên tắc này, bạn có thực sự cần Agentic Loop hay LLM Feature đủ?
```

**AI trả lời:**

✅ **Chính xác lần 2:** "LLM Feature đủ. Agentic Loop là overkill vì:
- Quy trình tuyến tính, không có decision tree phức tạp.
- Dispatcher luôn review → không cần agent tự quyết định.
- Complexity không justify được benefit."

**Bài học:** Giả sử **scope ngay từ đầu** sẽ giúp AI không đề xuất over-engineered solution.

---

### Sai Sót #2: Con Số Metric Quá Lạc Quan

**Prompt ban đầu:**

```
Bạn hãy ước tính metric thành công cho bài toán này:
1. Giảm thời gian tìm khách
2. Tỉ lệ landmark chính xác
3. Độ hài lòng tài xế
```

**AI trả lời:**

```
Metric đề xuất:
1. Giảm từ 2-3 phút → dưới 30 giây (90% improvement)
2. 98% landmark chính xác
3. NPS tài xế tăng từ 60 → 85

Thời gian đạt: 2-3 tuần test
```

⚠️ **Quá lạc quan.**

**Tôi sửa prompt:**

```
Hãy bảo thủ hơn:
1. Giảm thời gian từ 2-3 phút → dưới 45-60 giây (chứ không phải 30s)
   Lý do: Dispatcher vẫn cần 5-10s để review draft
2. ≥90% landmark (chứ không phải 98%)
   Lý do: POI DB có thể incomplete, khách mô tả ambiguous
3. Không claim NPS ngay - focus vào objective metrics (thời gian, error rate)
   Lý do: NPS phụ thuộc quá nhiều yếu tố khác (app UX, giá cước, etc.)

Timeline: 2-4 tuần test (2 tuần đầu fine-tune, 2 tuần test A/B)
```

**AI trả lời:**

✅ **Thực tế hơn:** "
1. 45-60 giây là achievable nếu POI DB đủ sạch.
2. 90% là realistic goal với proper validation.
3. Nên measure: Error rate (% landmark sai), Cost per transaction (dispatcher time).
"

**Bài học:** Khi AI đề xuất metric quá lạc quan → **challenge nó** với conservative estimate. CFO sẽ hỏi "worst-case scenario là gì?" → phải prepared.

---

## 🎯 Phần 4: Các Lần Tôi Không Dùng AI (Và Tại Sao)

### Không Dùng AI Cho: Problem Statement 6-Field

**Lý do:**
- Đây là **kiến thức nội bộ** về Xanh SM (Actor, Current workflow, Business impact) → Tôi phải phỏng vấn/brainstorm với team Xanh SM, không phải hỏi AI.
- AI có thể halucinate về số liệu nội bộ.

**Cách tôi làm:** Tự viết dựa trên **Card #1 đã sketched ở Phase 2** (vì đó đã là tóm tắt của brainstorm thực tế với stakeholder).

---

### Không Dùng AI Cho: Tính Toán Chi Phí Chính Xác

**Lý do:**
- "Giảm thời gian từ 2 phút → 45 giây" × 300 cuốc/ngày = ??? tiền.
- AI sẽ hallucinate về "giá trị giờ tài xế", "chi phí sạc điện", "đâu là profit margin".
- Chỉ CFO / Kế toán Xanh SM biết.

**Cách tôi làm:** Viết *"Giảm 1.5 phút × 300 cuốc/ngày = 450 phút/ngày tiết kiệm"* → **Để CFO tính tiền**. Vì tôi không biết giá trị của "1 phút tài xế" trong model kinh doanh Xanh SM.

---

## 💡 Kết Luận & Feedback Cho Các Bạn

### Khi Nên Dùng AI

✅ **Brainstorm ý tưởng:** Nhưng phải verify với stakeholder thực.  
✅ **Thiết kế workflow:** Vẽ diagram, xác định bottleneck.  
✅ **Thiết kế validation/fallback:** AI giỏi tư duy "điều gì có thể sai?".  
✅ **So sánh kiến trúc:** Rule vs LLM vs Agent — AI có intuition tốt.  
✅ **Sửa prompt:** Nếu AI trả lời sai → challenge nó, cho context lại.

### Khi KHÔNG Nên Dùng AI

❌ **Số liệu nội bộ:** Business Impact, tổn thất tiền tệ chính xác.  
❌ **Stakeholder input:** Ai đang đau, workflow thực tế → phải phỏng vấn.  
❌ **Quyết định cuối:** GO/NOT YET/NO-GO → của Ban Giám Đốc, không phải AI.

### Prompt Engineering Best Practice

1. **Cung cấp context từ đầu** — không phải hỏi "hãy chọn" mà phải nói "tôi muốn showcase AI bằng bài toán nào?"
2. **Challenge khi AI lạc quan** — "Metric này có realistic không? Worst-case?"
3. **Flag giả định** — "Số liệu này là ước tính cần verify" → Rõ ràng ở report.
4. **Không phụ thuộc AI cho decision** — AI là thought-partner, không phải decision-maker.

---

**Ký tên:**  
Tôi (hunglm)  
Vin Smart Future  
11.09.2026
