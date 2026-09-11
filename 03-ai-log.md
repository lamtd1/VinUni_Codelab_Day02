# 📝 Nhật Ký Tương Tác AI & Bài Học Phản Ánh (AI Log & Reflection) — Lab 02

> **Học viên thực hiện:** Nguyễn Xuân Khuê  
> **Vị trí:** AI Engineer — Vin Smart Future (Vingroup)  
> **Dự án Deep-Dive:** Hệ thống phân loại, điều hướng ticket khiếu nại và gợi ý phản hồi cá nhân hóa cư dân trên App Vinhomes Resident (Card #1)  
> **Mô hình AI sử dụng:** Google Gemini 2.5 Flash / Gemini Pro  

---

## 🏛️ Phần 1: Bối cảnh & Phương pháp tiếp cận (AI Co-pilot Mindset)

Trong buổi Lab 02 về **AI Product Scoping**, tôi sử dụng các mô hình ngôn ngữ lớn (LLMs) như một **trợ lý đồng hành (Co-pilot & Thought Partner)** thay vì một cỗ máy tự động ra quyết định thay con người.

Mục tiêu của tôi khi làm việc với AI là:
1. **Mở rộng góc nhìn:** Quét nhanh các điểm nghẽn vận hành (bottlenecks) qua 4 lăng kính (Lặp lại, Tốn thời gian, AI-upgrade, Pain từ người khác) trong hệ sinh thái Vingroup.
2. **Thử thách giả định (Stress-testing):** Yêu cầu AI đóng vai các bên liên quan khắt khe (CFO, Giám đốc Vận hành BQL Vinhomes) để chỉ ra điểm yếu trong bản đề xuất.
3. **Thiết lập ranh giới an toàn (Operational Boundaries):** Khám phá những tình huống biên (edge cases) và kiểm tra khả năng kiểm soát rủi ro thông qua prompt engineering.

---

## 💬 Phần 2: Nhật ký tương tác Prompt & Phản hồi thực tế (Prompt Log)

Dưới đây là các prompt chính tôi đã sử dụng xuyên suốt các giai đoạn của bài toán Vinhomes:

### Prompt 1 — Quét cơ hội & Lựa chọn bài toán (Phase 1 & 2)

* **Prompt:**
  ```text
  Tôi là AI Engineer tại Vin Smart Future, đang khảo sát quy trình vận hành tại các công ty 
  thành viên Vingroup (Vinhomes, VinFast, Xanh SM, Vinmec). 
  Hãy đóng vai Trưởng bộ phận Vận hành Vinhomes, liệt kê 3 bài toán xử lý phản hồi/khiếu nại 
  của cư dân đang tốn nhiều thời gian và nhân lực nhất hiện nay. 
  Với mỗi bài toán, chỉ rõ: Ai đang thực hiện, khâu nào bị nghẽn (bottleneck), và tại sao 
  phương pháp Rule-based thông thường lại thất bại.
  ```

* **Phân tích kết quả:**
  - ✅ **Điểm tốt:** AI chỉ ra chính xác vấn đề ở khâu đọc hiểu phản ánh của cư dân. Cư dân không bao giờ điền đúng form mẫu mà thường viết tự do, dùng từ lóng hoặc gộp nhiều vấn đề vào một tin nhắn.
  - ⚠️ **Hạn chế:** AI ban đầu đề xuất ý tưởng "Voicebot tự động gọi lại giải thích cho cư dân", điều này quá tốn kém và dễ gây phản cảm nếu cư dân đang bực mình.
  - 🔄 **Hành động điều chỉnh:** Tôi giữ lại bài toán phân loại & điều hướng (Ticket Routing) kết hợp sinh draft câu trả lời cho nhân viên CSKH duyệt, loại bỏ ý tưởng gọi điện thoại tự động.

---

### Prompt 2 — Chuẩn hóa 6-Field Problem Statement (Phase 3)

* **Prompt:**
  ```text
  Tôi muốn hoàn thiện 6-field Problem Statement cho bài toán CRM Vinhomes:
  1. Actor: CSKH trực hệ thống CRM Vinhomes
  2. Current Workflow: Nhận ticket -> Đọc hiểu -> Gán tag -> Route tổ đội -> Phản hồi xác nhận
  3. Bottleneck: Khâu đọc hiểu và gán nhãn (tốn 3-5 phút/ticket vì đa ý định)
  4. Business Impact: Quá tải giờ cao điểm, vi phạm cam kết SLA (FRT > 15 phút), giảm điểm CSAT

  Hãy đóng vai một CFO khắt khe:
  - Chỉ ra Success Metric nào đo lường được bằng con số cụ thể, tránh dùng từ chung chung.
  - Đặt ra Operational Boundary nghiêm ngặt nhất để tránh rủi ro pháp lý/truyền thông cho Vinhomes.
  ```

* **Phân tích kết quả:**
  - ✅ **Điểm tốt:** AI đã gợi ý chia ranh giới thành 3 nhóm rõ rệt: *Được phép làm*, *Tuyệt đối cấm*, và *Điểm bắt buộc con người phê duyệt (HITL)*.
  - ✅ **Kết quả đạt được:** Thành hình trường số 5 (Metric: Routing Accuracy > 85%, FRT < 1 phút) và trường số 6 (Boundary: Không auto-reply khiếu nại tài chính/pháp lý).

---

### Prompt 3 — Thiết kế Future-State Flow & Fallback Logic (Phase 3.3)

* **Prompt:**
  ```text
  Thiết kế luồng vận hành tương lai (Future-State Flow) khi tích hợp LLM vào CRM Vinhomes:
  - Đầu vào: Ticket cư dân gửi từ App (văn bản phi cấu trúc).
  - Đầu ra mong muốn: JSON gồm {"intent": ..., "urgency": ..., "category": ..., "draft_response": ...}.

  Hãy thiết kế cơ chế Fallback:
  Trong trường hợp nào hệ thống phải bypass AI hoàn toàn để đẩy thẳng cho Quản lý CSKH xử lý thủ công?
  ```

* **Phân tích kết quả:**
  - ✅ **Điểm tốt:** AI đề xuất 2 trường hợp Fallback rất sắc sảo:
    1. **Từ khóa nhạy cảm / Khẩn cấp:** Chứa từ khóa liên quan đến pháp lý, kiện tụng, báo chí, cơ quan chức năng, hoặc đe dọa an toàn tính mạng.
    2. **Độ tin cậy thấp (Low Confidence):** Khi điểm tin cậy của mô hình dưới 70%.

---

## 🚨 Phần 3: Các sai sót / Hallucination của AI & Cách tôi sửa Prompt

Trong quá trình làm việc, AI đã gặp phải một số lỗi tư duy và ảo tưởng (hallucination) quan trọng. Tôi đã trực tiếp phản biện và sửa prompt như sau:

### 1. Sai sót #1: AI đề xuất tự động gửi phản hồi trực tiếp (Full Automation)

* **Biểu hiện của AI:** Ở bản phác thảo đầu tiên, AI đề xuất sau khi mô hình sinh câu trả lời thì hệ thống CRM sẽ tự động gửi thẳng tin nhắn phản hồi tới App của cư dân để đạt thời gian phản hồi là 0 giây.
* **Tại sao sai?** Đây là lỗi tư duy nghiêm trọng về an toàn sản phẩm. Trong dịch vụ bất động sản cao cấp như Vinhomes, ngôn từ phản hồi cư dân đòi hỏi sự thấu cảm, chuẩn mực và chính xác tuyệt đối. Nếu LLM hallucinate ra thông tin sai (ví dụ: hứa hẹn bồi thường tiền điện, cam kết sửa xong trong 1 giờ trong khi thợ chưa có mặt), Vinhomes sẽ đối mặt với khủng hoảng truyền thông và khiếu nại pháp lý.
* **Cách tôi sửa:**
  - Bắt buộc bổ sung cơ chế **Human-in-the-loop (HITL)**: AI chỉ dừng ở vai trò trợ lý soạn nháp (drafting).
  - Bổ sung quy tắc ranh giới cứng: Bắt buộc mọi output văn bản gửi cư dân phải có tiền tố `[DRAFT_ONLY]` để hệ thống CRM nhận diện và khóa nút gửi tự động, chỉ mở khi nhân viên CSKH bấm duyệt.

---

### 2. Sai sót #2: Đề xuất kiến trúc Agentic Loop quá phức tạp (Over-Engineering)

* **Biểu hiện của AI:** Khi được hỏi về kiến trúc hệ thống, AI đề xuất mô hình gồm 4 Agent độc lập giao tiếp qua lại: *Triage Agent*, *Sentiment Analyzer Agent*, *Policy Checker Agent*, và *Drafting Agent*.
* **Tại sao sai?**
  - Bài toán phân loại và soạn draft ticket tại Vinhomes là một quy trình có cấu trúc cố định, dữ liệu đi tuần tự một chiều.
  - Dùng Multi-Agent Loop làm tăng độ trễ (latency) từ 2 giây lên 15–20 giây, chi phí token nhân lên gấp 4 lần, và cực kỳ khó debug khi xảy ra lỗi.
* **Cách tôi sửa:** Tôi yêu cầu AI quay về ma trận **AI-Fit Matrix** và chọn **LLM Feature**. Một lệnh gọi Gemini với cấu trúc đầu ra có định dạng (Structured Outputs JSON) kèm System Prompt chặt chẽ hoàn toàn giải quyết được toàn bộ các yêu cầu trên trong 1 lần suy luận duy nhất (Single inference).

---

### 3. Sai sót #3: Ước tính Success Metric quá lạc quan và thiếu căn cứ

* **Biểu hiện của AI:** AI tự tin khẳng định: *"Đạt độ chính xác phân loại 99.5% và giải quyết tự động 100% khiếu nại của cư dân sau 2 tuần triển khai"*.
* **Tại sao sai?** Trong thực tế, dữ liệu khiếu nại của cư dân chứa nhiều tiếng lóng vùng miền, văn phong bức xúc, ảnh chụp mờ hoặc đa ý định đan xen. Con số 99.5% là hoàn toàn bất khả thi đối với bài toán NLP thực tế.
* **Cách tôi sửa:** Tôi yêu cầu AI hạ mục tiêu xuống mức thực tế dựa trên baseline của ngành chăm sóc khách hàng:
  - Routing Accuracy ban đầu: **> 85%**.
  - First Response Time (FRT): Giảm từ trung bình **5 phút xuống dưới 1 phút/vé**.
  - Luôn duy trì tỷ lệ con người can thiệp (HITL) là 100% đối với các ticket gửi ra ngoài.

---

## 🛑 Phần 4: Những khâu tôi KHÔNG dùng AI (Và lý do)

Có những khâu trong quá trình làm báo cáo tôi chủ động không sử dụng AI:

1. **Ước lượng thiệt hại tài chính và chi phí vận hành:**
   - *Lý do:* AI không nắm được cơ cấu lương nhân sự CSKH tại các khu đô thị Vinhomes, cũng như chi phí phạt vi phạm cam kết dịch vụ nội bộ. Dùng AI tính toán sẽ dẫn đến số liệu "ảo". Tôi để các trường này ở dạng thời gian (giờ công/tháng, phút/lượt) để chuyển giao cho bộ phận Tài chính/Vận hành tính toán chính xác.
2. **Quyết định ranh giới cấm (Operational Boundaries):**
   - *Lý do:* Ranh giới pháp lý (không cam kết bồi thường, không trả lời tranh chấp diện tích sổ hồng) đòi hỏi sự hiểu biết sâu sắc về quy định của Vinhomes và pháp luật Việt Nam. Đây là trách nhiệm của con người, không thể giao phó cho mô hình ngôn ngữ.
3. **Quyết định phê duyệt dự án (GO / NO-GO):**
   - *Lý do:* Quyết định đầu tư vào dự án công nghệ phụ thuộc vào mức độ sẵn sàng của nhân sự và định hướng chiến lược của Ban Giám đốc Vin Smart Future, AI chỉ đóng vai trò phân tích dữ kiện hỗ trợ.

---

## 💡 Phần 5: Bài học kinh nghiệm & Đúc kết về AI Co-pilot

1. **Prompt Engineering chính là Boundary Engineering:** Một prompt tốt không chỉ hướng dẫn AI làm cái gì, mà quan trọng hơn là **ngăn chặn AI không được làm những cái gì** (Operational Boundaries).
2. **Luôn bắt đầu từ kiến trúc đơn giản nhất:** Tránh bẫy "Agentic Hype". Nếu Rule-based giải quyết được, dùng Rule. Nếu 1 LLM Call giải quyết được, dùng LLM Feature. Chỉ dùng Agent khi thực sự cần tính tự trị và khả năng tương tác môi trường đa bước.
3. **Human-in-the-loop là chìa khóa triển khai AI vào thực tế:** Đối với các tập đoàn lớn như Vingroup, bảo vệ uy tín thương hiệu luôn là ưu tiên hàng đầu. Giữ con người ở vị trí chốt chặn cuối cùng vừa đảm bảo an toàn tuyệt đối, vừa giúp AI học hỏi liên tục từ phản hồi của nhân viên vận hành.

---

**Người viết nhật ký:**  
**Nguyễn Xuân Khuê**  
*AI Engineer — Vin Smart Future*  
*Ngày hoàn thành: 11/09/2026*
