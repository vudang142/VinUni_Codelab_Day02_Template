# 📝 Nhật Ký Tương Tác AI (AI Interaction Log)
**Họ và tên:** Lưu Nguyễn Tiến Anh  
**Dự án:** Trợ lý AI thẩm định & đối soát bảo hành xe điện VinFast  

---

## 1. AI giúp ích được gì?
* **Brainstorm đa chiều (Phase 1):** Gợi ý các điểm nghẽn thực tế từ góc nhìn nhân viên thực địa (KTV xưởng, tài xế Xanh SM) thay vì chỉ nhìn từ cấp quản lý.
* **Chuẩn hóa khung tư duy (Phase 2 & 3):** Ép bài toán vào format 6-field tinh gọn, bóc tách chính xác các nút thắt 🔴 Bottleneck và điểm bàn giao 🔄 Handoff.
* **Định hình kiến trúc:** Gợi ý mô hình lai giữa Parser dữ liệu kỹ thuật và Agentic RAG tra cứu sổ tay bảo hành.

---

## 2. AI sai sót / Hallucination ở đâu?
* **Ảo giác quyền hạn (Over-automation):** Tự ý đề xuất cho AI *tự duyệt lệnh xuất kho phụ tùng/đổi pin mới*. Điều này vi phạm nghiêm trọng quy chuẩn tài chính vì giá trị bộ pin rất lớn, nguy cơ thất thoát nếu có gian lận.
* **Đơn giản hóa dữ liệu đầu vào:** Tưởng nhầm dữ liệu chẩn đoán chỉ là vài dòng text ngắn, trong khi thực tế log cổng OBD/CAN-bus là file hex dump/JSON dài hàng nghìn dòng.
* **Bỏ qua điều kiện loại trừ:** Chỉ tra cứu theo mã lỗi DTC mà bỏ quên các điều khoản pháp lý loại trừ (độ chế phần mềm, sạc sai cách, bảo dưỡng trễ hạn).

---

## 3. Cách tinh chỉnh Prompt & Ranh giới vận hành
* **Siết ranh giới con người (HITL):** Ép prompt giới hạn AI chỉ làm Co-pilot (bóc tách log, tra cứu tài liệu, soạn draft claim và tính Risk Score). Con người (KTV và Chuyên viên HQ) nắm 100% quyền bấm phê duyệt cuối cùng.
* **Tách lớp kiến trúc Hybrid:** Bắt buộc AI phân tầng rõ ràng: dùng Code/Parser tất định để xử lý file log thô thành dữ liệu sạch trước khi đẩy vào LLM/RAG để sinh văn bản.
* **Dùng Red Teaming Prompt:** Đóng vai *"CFO & Trưởng phòng Hậu mãi khắt khe"* để AI tự phản biện lỗ hổng logic, bổ sung ngưỡng tin cậy `< 85%` để kích hoạt cơ chế ↩️ Fallback chuyển người xử lý.

---

## 4. Đúc kết
AI giúp tăng tốc tổng hợp và cấu trúc hóa bài toán cực nhanh, nhưng dễ mắc bẫy tự động hóa quá đà và thiếu nhạy bén về rủi ro ngân sách. Cốt lõi của giải pháp AI trong doanh nghiệp là: **Dữ liệu phải tiền xử lý bằng code chuẩn, quy trình phải có Human-in-the-Loop, và ranh giới vận hành phải đóng khung tuyệt đối.**