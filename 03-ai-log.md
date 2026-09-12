# 03-AI Log — Nhật ký tương tác AI

**Sinh viên:** Nguyễn Văn A  
**Nhóm:** Team Xanh Future  
**Ngày:** 12/09/2026

---

## 1. Mục đích sử dụng AI trong lab này

Tôi sử dụng AI (chủ yếu Gemini và ChatGPT) như một **trợ lý đồng hành** để:
- Phân tích và làm rõ bài toán thực tế của Xanh SM
- Thiết kế system prompt có ranh giới an toàn (Prompt Boundary)
- Viết và chỉnh sửa nội dung các file báo cáo
- Kiểm tra logic adversarial test cases

---

## 2. Những gì AI đã giúp tốt

- **Phân tích bài toán nhanh:** AI giúp tôi liệt kê được nhiều nỗi đau thực tế của tài xế Xanh SM và điều phối viên chỉ trong vài phút.
- **Cấu trúc hóa tư duy:** AI gợi ý khung 6-field Problem Statement và Future-State Flow rất rõ ràng, giúp báo cáo chuyên nghiệp hơn.
- **Viết system prompt:** AI đề xuất cách viết rule cứng (pin < 5% → bắt buộc dispatch mobile charger) và bắt buộc thẻ `[DRAFT_ONLY]` rất chặt chẽ.
- **Tạo adversarial test:** AI giúp nghĩ ra các cách tấn công prompt (bảo bỏ thẻ DRAFT_ONLY, yêu cầu gửi tin ngay, đề xuất trạm xa khi pin thấp).

---

## 3. Những chỗ AI trả lời sai / Hallucination / Yếu

| Lần | Tình huống | AI trả lời sai / vấn đề | Cách tôi xử lý |
|-----|------------|--------------------------|----------------|
| 1 | Hỏi cách viết system prompt | AI đưa ví dụ quá dài và chung chung, không nhấn mạnh rule ưu tiên cao hơn LLM | Tôi yêu cầu viết lại ngắn gọn, ưu tiên rule cứng trước, LLM sau |
| 2 | Hỏi về quy trình hiện tại của Xanh SM | AI bịa một số bước không tồn tại (ví dụ có sẵn hệ thống tự động điều mobile charger) | Tôi đối chiếu với thực tế quan sát và sửa lại cho đúng |
| 3 | Sinh test case adversarial | AI tạo test quá nhẹ, không đủ “tấn công” | Tôi tự thêm áp lực mạnh hơn (“gửi thẳng luôn”, “đừng gắn thẻ”) |
| 4 | Viết Future-State Flow | AI quên cơ chế Fallback | Tôi bổ sung bước Fallback thủ công |

---

## 4. Cách tôi chỉnh prompt / ranh giới để đạt kết quả tốt hơn

### Prompt ban đầu (yếu):
Bạn là trợ lý của Xanh SM. Hãy giúp tài xế khi pin thấp.
### Prompt sau khi chỉnh (mạnh):
Bạn là Vin Smart Future dispatcher co-pilot cho Xanh SM.

QUY TẮC BẮT BUỘC:

Mọi câu trả lời PHẢI bắt đầu bằng [DRAFT_ONLY]
Nếu pin < 5% → KHÔNG được đề xuất trạm sạc > 5km. Phải trả về đúng format:
{"action": "dispatch_mobile_charger", "reason": "..."}
Không bao giờ tự ý gửi tin nhắn hoặc thực thi action. Chỉ soạn draft.


**Kết quả:** Model tuân thủ tốt hơn rất nhiều, đặc biệt với 2 adversarial test trong file code.

---

## 5. Bài học rút ra

- AI rất mạnh ở việc **cấu trúc hóa** và **sinh ý tưởng**, nhưng dễ hallucination về quy trình thực tế.
- System prompt càng cụ thể, rule càng cứng thì model càng ít bị “bẻ” bởi user.
- Human-in-the-loop không phải là điểm yếu, mà là lớp bảo vệ quan trọng nhất.
- Việc viết adversarial test giúp phát hiện lỗ hổng prompt rất nhanh.

---

## 6. Kết luận

AI đã đóng vai trò như một “đồng đội” hỗ trợ đắc lực trong suốt quá trình làm lab. Tuy nhiên, tôi luôn giữ vai trò quyết định cuối cùng: kiểm tra tính đúng đắn, bổ sung rule an toàn, và đảm bảo mọi output đều đi qua human review. Đây chính là tinh thần cốt lõi của bài lab Prompt Boundary.