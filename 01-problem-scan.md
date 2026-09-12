# 01 — Problem Scan: AI Product Scoping (Vin Smart Future)

> Deliverable cá nhân — Phase 1 (SCAN) & Phase 2 (QUICK-ASSESS)
> Người thực hiện: DamVietHung-02600 — Vai trò: AI Product Engineer, Vin Smart Future

---

# 🔍 Phase 1 — SCAN

Sử dụng 4 Lenses (Lặp lại / Tốn thời gian / AI-upgrade / Pain từ người khác) để quét vận hành của 5 công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|----------------------|
| 1 | **Vinmec** | Tốn thời gian | Bác sĩ nội trú soạn thủ công bản tóm tắt hồ sơ xuất viện (discharge summary) từ dữ liệu rải rác trên hệ thống HIS, mất 20–30 phút/ca. |
| 2 | **Vinhomes** | Lặp lại | Nhân viên tổng đài phân loại và điều hướng thủ công hàng trăm phản ánh cư dân/ngày (mất nước, hỏng đèn, ồn ào...) trên App Vinhomes Resident đến đúng ban quản lý toà nhà. |
| 3 | **VinFast** | AI có thể tốt hơn | Tư vấn viên tổng đài phải tự diễn giải mô tả lỗi xe bằng tiếng Việt tự nhiên của khách hàng (VD: "xe qua gờ giảm tốc kêu cụp cụp") để gán mã lỗi kỹ thuật ban đầu, dễ gán sai mã do thiếu kinh nghiệm. |
| 4 | **Vinpearl** | Pain từ người khác | Quản lý khách sạn phàn nàn vì phải tự đọc thủ công hàng trăm review/ngày trên Booking.com, Agoda, Google Maps để phát hiện các phàn nàn khẩn cấp (phòng bẩn, thái độ nhân viên). |
| 5 | **Xanh SM** | Lặp lại | Đội vận hành nghe lại thủ công ghi âm cuộc gọi + ghi chú tài xế để tổng hợp lý do hủy chuyến, phục vụ phân tích nguyên nhân gốc hàng tuần. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Top 3 bài toán được chọn từ danh sách SCAN: **#1 (Vinmec)**, **#2 (Vinhomes)**, **#3 (VinFast)**.

## Card #1 — Vinmec: Soạn thảo tóm tắt hồ sơ xuất viện

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                        │
│                                                               │
│ Bài toán: Bác sĩ nội trú mất nhiều thời gian tổng hợp dữ liệu│
│ lâm sàng và soạn bản tóm tắt xuất viện dễ hiểu cho bệnh nhân.│
│ Công ty thành viên: [x] Vinmec                               │
│                                                               │
│ Ai đang đau (Actor)? Bác sĩ điều trị / bác sĩ nội trú.       │
│                                                               │
│ Workflow thủ công hiện tại (4 bước):                         │
│   1. Tra cứu HIS (chẩn đoán, xét nghiệm, chỉ định)           │
│   → 2. Tổng hợp diễn biến điều trị thành văn bản              │
│   → 3. Viết lại bằng ngôn ngữ dễ hiểu cho bệnh nhân           │
│   → 4. Bác sĩ trưởng khoa ký duyệt trước khi phát hành        │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (⏱ 18 phút/lượt)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–3 (soạn draft)  │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Giảm thời gian soạn thảo từ 25 phút ──> dưới 5 phút.       │
│                                                               │
│ Quick Architecture: [x] LLM Feature (bắt buộc HITL)          │
└─────────────────────────────────────────────────────────────┘
```

## Card #2 — Vinhomes: Phân loại & điều hướng phản ánh cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                        │
│                                                               │
│ Bài toán: Phản ánh cư dân trên App Vinhomes Resident bị      │
│ phân loại và điều hướng thủ công đến sai ban quản lý.        │
│ Công ty thành viên: [x] Vinhomes                             │
│                                                               │
│ Ai đang đau? Nhân viên tổng đài (quá tải), cư dân (chờ lâu)  │
│                                                               │
│ Workflow thủ công hiện tại (4 bước):                         │
│   1. Nhận phản ánh qua App                                   │
│   → 2. Đọc & phân loại thủ công theo hạng mục                │
│   → 3. Tra cứu ban quản lý phụ trách toà/hạng mục đó         │
│   → 4. Chuyển ticket + theo dõi SLA phản hồi                  │
│                                                               │
│ Bước nào tốn nhất? Bước 2–3 (⏱ 8 phút/lượt, x300 ticket/ngày)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–3               │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                        │
│   85% ticket được phân loại & route đúng trong dưới 10 giây. │
│                                                               │
│ Quick Architecture: [x] Rule + LLM Feature (hybrid classifier)│
└─────────────────────────────────────────────────────────────┘
```

## Card #3 — VinFast: Gán mã lỗi kỹ thuật ban đầu từ mô tả khách hàng

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                        │
│                                                               │
│ Bài toán: Tư vấn viên tổng đài gán sai mã lỗi kỹ thuật ban   │
│ đầu khi khách mô tả sự cố xe bằng ngôn ngữ tự nhiên.         │
│ Công ty thành viên: [x] VinFast                              │
│                                                               │
│ Ai đang đau? Tư vấn viên (thiếu kinh nghiệm kỹ thuật),       │
│ trung tâm dịch vụ (nhận sai lịch hẹn/phụ tùng chuẩn bị sai). │
│                                                               │
│ Workflow thủ công hiện tại (4 bước):                         │
│   1. Khách mô tả triệu chứng qua hotline/chat                │
│   → 2. Tư vấn viên tra bảng mã lỗi thủ công                  │
│   → 3. Gán mã lỗi sơ bộ + đặt lịch trung tâm dịch vụ         │
│   → 4. Kỹ thuật viên xác nhận lại khi xe đến xưởng           │
│                                                               │
│ Bước nào tốn/lỗi nhất? Bước 2 (⏱ 6 phút, tỉ lệ gán sai ~30%) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (gợi ý mã lỗi)  │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Giảm tỉ lệ gán sai mã lỗi từ 30% xuống dưới 10%.           │
│                                                               │
│ Quick Architecture: [x] LLM Feature (gợi ý, không tự quyết)  │
└─────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn của nhóm

Nhóm chọn **Card #1 — Vinmec: Soạn thảo tóm tắt hồ sơ xuất viện** để thực hiện Deep-Dive.

**Lý do loại các thẻ còn lại:**
- **Card #2 (Vinhomes):** Bài toán rõ nhưng độ phức tạp AI thấp — phần lớn có thể giải quyết bằng rule-based router theo từ khóa/hạng mục, chưa đủ chiều sâu để phân tích Rule vs LLM vs Agent.
- **Card #3 (VinFast):** Rủi ro an toàn kỹ thuật cao (gán sai mã lỗi có thể ảnh hưởng an toàn vận hành xe điện) nhưng nhóm chưa tiếp cận được dữ liệu bảng mã lỗi thực tế của VinFast để scoping chính xác — cần thêm thời gian thu thập dữ liệu trước khi Deep-Dive.
- **Card #1 (Vinmec)** được chọn vì: có metric đo lường rõ ràng (thời gian xử lý), ranh giới an toàn (Operational Boundary) rất rõ và mang tính giáo dục cao khi thiết kế Human-in-the-loop bắt buộc, đồng thời tác động kinh doanh (business impact) đo được cụ thể qua số ca xuất viện/ngày.
