# 01-Problem Scan — Vin Smart Future Lab
 

---

## Phase 1: SCAN — Bảng quét cơ hội (≥ 5 bài toán)

| # | Công ty Vingroup | Bài toán / Nỗi đau thực tế | Actor chính | Quy trình hiện tại (tóm tắt) | Bottleneck / Điểm nghẽn | Cơ hội AI (ý tưởng sơ bộ) | Mức độ cấp thiết (1-5) |
|---|---------------|-----------------------------|-------------|------------------------------|--------------------------|---------------------------|------------------------|
| 1 | Xanh SM | Tài xế VF8/VF9 báo pin thấp (<5%) nhưng hệ thống vẫn đề xuất trạm sạc xa (>8–10km) | Tài xế Xanh SM + Điều phối viên | Tài xế gửi tin nhắn/app → Điều phối viên kiểm tra thủ công → Gửi chỉ đường | Mất thời gian kiểm tra + rủi ro xe chết máy giữa đường | AI tự động phát hiện pin <5% → bắt buộc trigger mobile charger, không cho đề xuất trạm xa | 5 |
| 2 | Xanh SM | Soạn tin nhắn chăm sóc khách hàng (cảm ơn, xin lỗi, hướng dẫn) vẫn phải viết tay, dễ sai sót và chậm | Điều phối viên / CSKH | Nhận yêu cầu → Viết tin nhắn → Gửi trực tiếp | Thời gian soạn tin lâu, không có bước review chuẩn | AI soạn draft tin nhắn + bắt buộc gắn thẻ [DRAFT_ONLY] để human review trước khi gửi | 4 |
| 3 | VinFast / Xanh SM | Dự đoán nhu cầu sạc pin theo khu vực và khung giờ để bố trí mobile charger trước | Đội vận hành sạc + Điều phối | Dựa vào kinh nghiệm + báo cáo thủ công cuối ngày | Thiếu dữ liệu real-time → mobile charger đến muộn hoặc thừa | AI dự báo nhu cầu sạc theo GPS + lịch sử + thời tiết → gợi ý vị trí standby | 4 |
| 4 | Xanh SM | Tài xế gặp sự cố kỹ thuật (lỗi BMS, không sạc được) phải gọi điện thoại và chờ hỗ trợ lâu | Tài xế + Kỹ thuật viên | Gọi hotline → Mô tả lỗi → Kỹ thuật viên hỏi lại → Cử người | Thời gian chẩn đoán lâu, thông tin thiếu chính xác | AI thu thập thông tin lỗi qua hội thoại + gợi ý checklist chẩn đoán nhanh | 3 |
| 5 | Vinmec / Xanh SM (liên kết) | Khách hàng đặt xe đưa đón khám bệnh nhưng không biết xe nào phù hợp (có sạc, ghế đặc biệt…) | Khách hàng + Điều phối viên | Khách gọi/app → Điều phối hỏi thủ công → Chọn xe | Trải nghiệm chậm, dễ chọn sai loại xe | AI hỏi nhu cầu + tự gợi ý loại xe phù hợp + soạn tin xác nhận | 3 |
| 6 | VinPearl | Xe điện đưa đón khách resort hết pin đột ngột giữa đường | Tài xế resort + Điều phối | Báo cáo qua bộ đàm → Điều phối tìm xe thay | Phản ứng chậm, ảnh hưởng trải nghiệm khách | AI giám sát pin real-time + tự động đề xuất phương án thay thế ngay | 4 |

---

## Phase 2: QUICK-ASSESS — 3 Quick Problem Cards

### Card 1: Xử lý khẩn cấp khi pin xe < 5%

- **Actor (Ai đang gặp vấn đề?):**  
  Tài xế Xanh SM (đang chở khách hoặc đang trống) + Điều phối viên trung tâm.

- **Quy trình hiện tại (Current Process):**  
  1. Tài xế thấy pin thấp → nhắn tin/app báo cáo  
  2. Điều phối viên mở bản đồ kiểm tra trạm sạc gần nhất  
  3. Gửi chỉ đường hoặc gọi điện hướng dẫn  
  4. Nếu quá xa thì mới nghĩ đến mobile charger (thường muộn)

- **Bottleneck (Điểm nghẽn cổ chai):**  
  Điều phối viên không có rule cứng → dễ đề xuất trạm sạc 8–12km khi pin chỉ còn 2–3% → rủi ro xe chết máy + khách phàn nàn + tài xế stress.

- **AI Solution (Ý tưởng giải pháp AI):**  
  AI Co-pilot nhận thông tin pin + vị trí → nếu pin < 5% thì **bắt buộc** trả về action `dispatch_mobile_charger` và **cấm** đề xuất trạm > 5km. Mọi tin nhắn đều bắt đầu bằng `[DRAFT_ONLY]`.

- **Metric thành công (KPI đo lường):**  
  - Giảm 80% trường hợp đề xuất trạm sạc nguy hiểm khi pin < 5%  
  - Thời gian từ lúc báo pin thấp đến lúc mobile charger được trigger < 60 giây  
  - Giảm tỷ lệ xe chết máy giữa đường liên quan đến pin

- **Mức độ khả thi sơ bộ:** **Cao**  
  (Đã có sẵn dữ liệu pin + GPS, có thể prototype ngay bằng system prompt + Gemini)

---

### Card 2: Soạn tin nhắn chăm sóc khách hàng có kiểm soát

- **Actor:**  
  Điều phối viên / Nhân viên CSKH Xanh SM.

- **Quy trình hiện tại:**  
  1. Nhận yêu cầu (khách phàn nàn, cảm ơn, hỏi đường…)  
  2. Viết tin nhắn thủ công  
  3. Gửi thẳng cho khách (không có bước review chuẩn)

- **Bottleneck:**  
  Viết chậm, giọng điệu không đồng nhất, dễ gửi nhầm thông tin hoặc thiếu lịch sự → ảnh hưởng thương hiệu.

- **AI Solution:**  
  AI soạn draft tin nhắn chuyên nghiệp theo ngữ cảnh → **luôn** gắn thẻ `[DRAFT_ONLY]` ở đầu → Điều phối viên chỉ việc review và bấm gửi.

- **Metric thành công:**  
  - Giảm 60–70% thời gian soạn tin  
  - 100% tin nhắn đều có human review trước khi gửi  
  - Tăng điểm hài lòng về giao tiếp (CSAT)

- **Mức độ khả thi sơ bộ:** **Cao**  
  (Rất phù hợp với lab Prompt Boundary đang làm)

---

### Card 3: Dự báo và bố trí Mobile Charger theo nhu cầu

- **Actor:**  
  Đội vận hành sạc di động + Điều phối viên cấp cao.

- **Quy trình hiện tại:**  
  1. Cuối ngày xem báo cáo số lần hết pin  
  2. Dựa vào kinh nghiệm để bố trí xe sạc standby  
  3. Phản ứng khi đã có tài xế báo cáo

- **Bottleneck:**  
  Thiếu dự báo → mobile charger thường đến sau khi tài xế đã chờ lâu hoặc phải di chuyển xa.

- **AI Solution:**  
  AI phân tích lịch sử GPS + mức độ pin trung bình + khung giờ cao điểm + sự kiện → gợi ý vị trí và số lượng mobile charger cần standby trước 30–60 phút.

- **Metric thành công:**  
  - Giảm 40% thời gian chờ trung bình của tài xế khi hết pin  
  - Tăng tỷ lệ “mobile charger đến trước khi pin về 0%”

- **Mức độ khả thi sơ bộ:** **Trung bình**  
  (Cần thêm dữ liệu lịch sử, phức tạp hơn 2 bài toán trên)

---

## Ghi chú / Ý tưởng thêm

- **Ưu tiên deep-dive:** Card 1 và Card 2 vì vừa cấp thiết vừa prototype được ngay bằng system prompt (đúng tinh thần lab).
- Ràng buộc quan trọng: Mọi output của AI **phải** có `[DRAFT_ONLY]` và tuân thủ rule pin < 5%.
- Hướng phát triển tiếp: Kết hợp Rule-based + LLM + Human-in-the-loop.