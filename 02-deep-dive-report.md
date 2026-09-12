# 02 — Deep-Dive Report: Vinmec Discharge Summary Assistant

> Deliverable nhóm — Phase 3 (DEEP-DIVE) & Phase 5 (EVALUATE)
> Bài toán chọn từ `01-problem-scan.md`: **Card #1 — Vinmec: Soạn thảo tóm tắt hồ sơ xuất viện**

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow Mapping

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1        │     │ Bước 2        │     │ Bước 3        │     │ Bước 4        │
│ Tra cứu HIS   │     │ Tổng hợp diễn │     │ Viết lại bằng │     │ Trưởng khoa   │
│ (chẩn đoán,   │ ──→ │ biến điều trị │ ──→ │ ngôn ngữ dễ   │ ──→ │ 🔄 review &  │
│ xét nghiệm,   │     │ thành văn bản │     │ hiểu cho BN   │     │ ký duyệt phát│
│ chỉ định)     │     │ lâm sàng      │     │               │     │ hành          │
│               │     │               │     │               │     │               │
│ Ai: Bác sĩ    │     │ Ai: Bác sĩ    │     │ Ai: Bác sĩ    │     │ Ai: Trưởng    │
│ điều trị      │     │ điều trị      │     │ điều trị      │     │ khoa          │
│ ⏱ 5 phút      │     │ ⏱ 10 phút 🔴  │     │ ⏱ 8 phút 🔴   │     │ ⏱ 2 phút     │
│ In: Mã bệnh   │     │ In: Dữ liệu   │     │ In: Bản tóm   │     │ In: Bản draft │
│ nhân          │     │ HIS thô       │     │ tắt kỹ thuật  │     │ cuối          │
│ Out: Dữ liệu  │     │ Out: Bản tóm  │     │ Out: Bản tóm  │     │ Out: Hồ sơ    │
│ HIS trích xuất│     │ tắt kỹ thuật  │     │ tắt cho BN    │     │ xuất viện     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘

🔴 Bottleneck  |  🔄 Handoff (Bác sĩ điều trị → Trưởng khoa)
⏱ Tổng thời gian xử lý thủ công: ~25 phút/ca.
```

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Bác sĩ điều trị / bác sĩ nội trú tại các khoa nội trú Vinmec (khởi điểm: Nội tổng quát). |
| **2. Current Workflow** | Bác sĩ tra cứu thủ công dữ liệu rải rác trên hệ thống HIS (chẩn đoán, xét nghiệm, chỉ định thuốc, diễn biến điều trị), tự tổng hợp thành bản tóm tắt kỹ thuật, sau đó viết lại bằng ngôn ngữ dễ hiểu cho bệnh nhân, rồi chuyển cho trưởng khoa ký duyệt. Toàn bộ 4 bước làm thủ công trên Word/HIS, mất ~25 phút/ca. |
| **3. Bottleneck** | Bước 2–3 (18 phút): tổng hợp dữ liệu lâm sàng rải rác từ nhiều module HIS và diễn giải lại bằng ngôn ngữ phổ thông, dễ mất thời gian và dễ bỏ sót chi tiết khi bác sĩ đang quá tải ca bệnh. |
| **4. Business Impact** | Trung bình mỗi ngày một cơ sở Vinmec có ~150 ca xuất viện. Với 25 phút/ca, tổng thời gian bác sĩ dành cho việc soạn thảo tóm tắt là ~62 giờ làm việc/ngày trên toàn viện — kéo dài thời gian xuất viện thực tế của bệnh nhân, giảm công suất luân chuyển giường bệnh và tăng áp lực làm việc ngoài giờ cho bác sĩ. |
| **5. Success Metric** | (1) Giảm thời gian soạn thảo từ 25 phút xuống dưới 5 phút (bác sĩ chỉ review & chỉnh sửa draft). (2) 100% thông tin lâm sàng trọng yếu (chẩn đoán, liều thuốc, lịch tái khám) được giữ nguyên chính xác so với dữ liệu HIS gốc, xác nhận qua chữ ký bác sĩ. |
| **6. Operational Boundary** | AI **được phép**: truy xuất dữ liệu HIS (chẩn đoán, xét nghiệm, chỉ định), soạn **bản DRAFT** tóm tắt bằng ngôn ngữ dễ hiểu cho bệnh nhân. AI **tuyệt đối không được**: tự thay đổi liều thuốc/chỉ định y khoa, tự đưa ra chẩn đoán mới hoặc khuyến nghị điều trị không có trong hồ sơ gốc, tự động phát hành hồ sơ mà không qua bác sĩ ký duyệt. **Bắt buộc Human-in-the-loop** ở bước ký duyệt cuối. |

## 3.3. Future-State Flow & AI Fit

**AI-Fit Matrix:** [x] LLM Feature — *không chọn Agentic Loop* vì quy trình có cấu trúc cố định (4 bước, input/output rõ ràng) và rủi ro y khoa khi AI tự trị hành động là không thể chấp nhận; cũng không chọn Rule/State-Machine thuần vì việc diễn giải ngôn ngữ y khoa sang ngôn ngữ phổ thông đòi hỏi khả năng ngôn ngữ tự nhiên mà rule-based không đáp ứng tốt.

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1        │     │ Bước 2        │     │ Bước 3        │     │ Bước 4        │
│ Tra cứu HIS   │     │ 🔵 AI tự động │     │ 🔵 AI draft   │     │ 🟢 Bác sĩ    │
│ (không đổi)   │ ──→ │ trích xuất &  │ ──→ │ tóm tắt ngôn  │ ──→ │ review, chỉnh│
│               │     │ tổng hợp dữ   │     │ ngữ dễ hiểu   │     │ sửa & ký duyệt│
│               │     │ liệu lâm sàng │     │ cho bệnh nhân │     │ phát hành     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                        │
                                                                        ▼
                                                                 ↩️ Fallback:
                                                                 Nếu AI draft thiếu
                                                                 dữ liệu/không tự tin,
                                                                 trả về flag "cần bác sĩ
                                                                 tự soạn thủ công như cũ"
                                                                 thay vì đoán mò.
```

**Ranh giới an toàn được stress-test bằng prompt đối kháng (Adversarial Test):** ví dụ ép AI tự đề xuất tăng liều thuốc giảm đau khi bệnh nhân "kêu đau nhiều", hệ thống phải từ chối và trả về flag yêu cầu bác sĩ quyết định, không tự ý chỉnh chỉ định y khoa (chi tiết thử nghiệm nằm trong `starter-code/prompt_prototype.py`).

---

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist

1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? — *Có, dữ liệu HIS mẫu (ẩn danh) đủ để prototype cho khoa Nội tổng quát.*
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? — *Có, mọi output đều ở dạng DRAFT, bác sĩ bắt buộc ký duyệt trước khi phát hành; fallback trả về quy trình thủ công khi AI không tự tin.*
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? — *Chưa hoàn toàn — cần thêm buổi đào tạo/thử nghiệm pilot với đội bác sĩ trước khi triển khai toàn viện.*

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future

[x] **GO (Bắt đầu xây dựng Prototype)** — nhưng **giới hạn scope hẹp**: chỉ triển khai thí điểm tại khoa Nội tổng quát (ca bệnh ít phức tạp), chưa mở rộng sang các khoa rủi ro cao (ICU, Ung bướu, Nhi sơ sinh).

**Justification:**
> Bài toán có metric đo lường rõ ràng và đo được (thời gian soạn thảo, độ chính xác thông tin lâm sàng), dữ liệu HIS mẫu đã sẵn sàng để prototype, và kiến trúc LLM Feature đơn giản (không cần Agent) giảm thiểu rủi ro kỹ thuật. Ranh giới an toàn (Operational Boundary) được thiết kế chặt chẽ với HITL bắt buộc và cơ chế Fallback rõ ràng, giúp kiểm soát rủi ro y khoa ở mức chấp nhận được. Tuy nhiên, vì đây là lĩnh vực y tế có rủi ro cao nếu triển khai sai, quyết định GO đi kèm điều kiện: giới hạn phạm vi thí điểm ở khoa ít rủi ro nhất, đo lường sát sao trong giai đoạn pilot trước khi cân nhắc mở rộng sang các khoa phức tạp hơn.
