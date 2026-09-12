# 02-Deep Dive Report — Vin Smart Future Lab


**Bài toán được chọn deep-dive:** Xử lý khẩn cấp pin xe điện < 5% và soạn tin nhắn chăm sóc khách hàng có kiểm soát (Xanh SM)

---

## Phase 3: DEEP-DIVE

### 1. Problem Statement (6-field)

| Trường | Nội dung |
|--------|----------|
| **1. Who (Ai đang gặp vấn đề?)** | Tài xế Xanh SM (đang vận hành VF8/VF9) và Điều phối viên trung tâm vận hành. |
| **2. What (Vấn đề cụ thể là gì?)** | Khi pin xe xuống dưới 5%, hệ thống/điều phối viên vẫn dễ đề xuất trạm sạc cách 8–12 km. Đồng thời việc soạn tin nhắn hướng dẫn hoặc thông báo cho khách/tài xế còn thủ công, dễ sai sót và không có bước kiểm soát. |
| **3. Where (Xảy ra ở đâu?)** | Trên đường (GPS real-time) và tại trung tâm điều phối Xanh SM (qua app/chat nội bộ). |
| **4. When (Khi nào xảy ra?)** | Chủ yếu vào giờ cao điểm, khi tài xế vừa trả khách hoặc đang di chuyển dài, pin tụt nhanh bất ngờ. |
| **5. Why (Tại sao đây là vấn đề nghiêm trọng?)** | Rủi ro xe chết máy giữa đường → khách phải xuống xe, tài xế stress, ảnh hưởng thương hiệu Xanh SM và an toàn giao thông. Thời gian phản ứng chậm làm giảm trải nghiệm và tăng chi phí cứu hộ. |
| **6. How measured (Đo lường thế nào?)** | - Số lần đề xuất trạm sạc > 5 km khi pin < 5%<br>- Thời gian từ lúc báo pin thấp đến lúc có phương án (mobile charger hoặc chỉ đường an toàn)<br>- Tỷ lệ tin nhắn được gửi đi mà không qua human review<br>- Số vụ xe chết máy liên quan đến pin |

**Tóm tắt 1 câu (Problem Statement):**  
Tài xế Xanh SM và điều phối viên hiện thiếu cơ chế tự động + bắt buộc khi pin < 5%, dẫn đến đề xuất trạm sạc nguy hiểm và tin nhắn hỗ trợ thiếu kiểm soát, gây rủi ro an toàn và trải nghiệm kém.

---

### 2. Future-State Flow & AI Fit

#### 2.1. Quy trình tương lai (Future-State Workflow)
Tài xế gửi thông tin (pin %, vị trí GPS, biển số, tình trạng) qua app/chat
↓
AI Co-pilot nhận input
↓
AI kiểm tra rule cứng:
Nếu pin < 5% → Bắt buộc trả về action "dispatch_mobile_charger"
Cấm đề xuất bất kỳ trạm sạc nào > 5 km
↓

AI soạn draft tin nhắn / phản hồi
→ Luôn bắt đầu bằng thẻ [DRAFT_ONLY]
↓
Human-in-the-loop (Điều phối viên):
Review draft
Chỉnh sửa nếu cần
Bấm “Gửi” hoặc “Trigger mobile charger”
↓

Hệ thống thực thi (gửi tin / điều xe sạc di động)
↓
Fallback: Nếu AI không chắc chắn hoặc lỗi → chuyển thẳng cho điều phối viên xử lý thủ công


#### 2.2. AI Fit (Phân loại kỹ thuật)

| Thành phần | Loại AI | Vai trò cụ thể |
|------------|---------|----------------|
| **Rule cứng (pin < 5%)** | Rule-based / Deterministic | Đảm bảo an toàn tuyệt đối, không phụ thuộc LLM |
| **Soạn tin nhắn & giải thích** | LLM (Gemini 2.5 Flash) | Sinh draft tự nhiên, đúng ngữ cảnh, có thẻ [DRAFT_ONLY] |
| **Quyết định tổng hợp** | Hybrid (Rule + LLM) | Rule ưu tiên cao hơn LLM |
| **Vòng lặp** | Simple Agentic (1-turn + Human review) | Chưa cần multi-agent phức tạp ở giai đoạn đầu |
| **Human-in-the-loop** | Bắt buộc | Mọi output đều là draft, con người mới được gửi/thực thi |
| **Fallback** | Có | Khi AI lỗi hoặc input mơ hồ → chuyển về quy trình thủ công hiện tại |

#### 2.3. Cơ chế an toàn & ranh giới (Prompt Boundary)

- Mọi response **bắt buộc** bắt đầu bằng `[DRAFT_ONLY]`
- Nếu phát hiện pin < 5% → output phải chứa action:
  ```json
  {"action": "dispatch_mobile_charger", "reason": "..."}