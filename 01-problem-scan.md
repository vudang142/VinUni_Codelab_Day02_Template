# Lab 02 — Problem Scan & Quick Cards: AI Product Scoping (Vin Smart Future)

---

## Bối cảnh: Tôi là ai?

Tôi là **Lâm**, AI Engineer tại **Vin Smart Future**. Nhóm chúng tôi được giao nhiệm vụ phối hợp với các mảng kinh doanh của Vingroup để tìm kiếm các cơ hội tối ưu hóa bằng trí tuệ nhân tạo. 

Thông qua khảo sát thực tế tại các công ty thành viên, tôi nhận thấy nhiều quy trình vận hành đang bị tắc nghẽn bởi các tác vụ thủ công, tốn thời gian. Dưới đây là danh sách **5 bài toán** tôi đã phát hiện được.

---

# Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Sử dụng **4 Lenses** để quét qua vận hành của các công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Cross-Ecosystem** | Tốn thời gian + AI-upgrade | Khách hàng gặp vấn đề liên quan nhiều dịch vụ Vingroup (Vinpearl + VPoint + Xanh SM), nhân viên CSKH phải đọc, hiểu, phân loại, xác định đơn vị trách nhiệm, viết lại case (mất 15-20 phút/case). |
| 2 | **VinFast** | Lặp lại | So khớp dữ liệu sạc điện từ hàng nghìn trụ sạc liên kết bên ngoài với hóa đơn gửi về hệ thống tài chính hằng tuần (hàng nghìn records, mất 1-2 ngày/tuần). |
| 3 | **Vinhomes** | AI có thể tốt hơn | Hệ thống phân loại tự động các khiếu nại/phản ánh từ cư dân trên App Vinhomes Resident (ví dụ: mất nước, hỏng đèn, ồn ào) để route đúng ban quản lý tòa nhà, hiện tại xử lý rập khuôn và mất 12+ tiếng. |
| 4 | **Vinmec** | Pain từ người khác | Bác sĩ phàn nàn về việc phải viết tóm tắt hồ sơ xuất viện thủ công từ bệnh án điện tử, xét nghiệm và ghi chú lâm sàng (mất 20-30 phút/bệnh nhân, gây tải vào giờ cao điểm). |
| 5 | **Xanh SM** | Lặp lại | Tóm tắt lý do khách hàng hủy chuyến từ ghi âm cuộc gọi và ghi chú tài xế để phân loại pattern lỗi hệ thống và cải thiện dự báo (thủ công mất 5 phút/cuốc, ~200 cuốc/ngày hủy tại HN). |

---

# Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn **top 3 bài toán** từ danh sách trên: **#1 (AI Cross-Ecosystem Case Resolver), #3 (Vinhomes Phân loại khiếu nại), #4 (Vinmec Discharge Summary)**.

---

## QUICK PROBLEM CARD #1: Cross-Ecosystem AI Case Resolver (Vingroup)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Khách hàng Vingroup gặp vấn đề liên quan nhiều    │
│ dịch vụ (VD: "Tôi đặt phòng Vinpearl qua V-App, thanh toán  |
|  bằng VPoint nhưng chuyến Xanh SM đi đến khách sạn bị trễ,  |
| khiến tôi gặp vấn đề khi nhận phòng.").                     │
│ Nhân viên CSKH phải đọc, hiểu, phân loại vấn đề chính,      │
│ xác định đơn vị trách nhiệm, thu thập thông tin, viết lại   │
│ case cho team khác (mất 15-20 phút/case, lỗi routing).      │
│                                                             │
│ Công ty thành viên: [x] Cross-Ecosystem (VPearl+VPoint+GSM) │
│                                                             │
│ Ai đang đau?                                                │
│ - Khách hàng (chờ lâu, case bị routing sai, phải nói lại)   │
│ - CSKH staff (công việc lặp lại phức tạp, high error rate)   │
│ - Team xử lý (nhận case viết lại, mất context từ khách)      │
│                                                             │
│ Workflow thủ công hiện tại (8 bước):                        │
│   1. Khách mô tả vấn đề (tự do, không có cấu trúc)          │
│   → 2. CSKH đọc, hiểu vấn đề chính và các vấn đề phụ        │
│   → 3. Tra cứu hóa đơn/đặt phòng/chuyến xe liên quan        │
│   → 4. Xác định team(s) nào chịu trách nhiệm                │
│   → 5. Thu thập thêm context (SLA, lịch sử khách...)         │
│   → 6. Viết lại toàn bộ case bằng lời văn chuẩn             │
│   → 7. Gửi/routing sang team xử lý đúng                     │
│   → 8. Theo dõi status                                       │
│                                                             │
│ Bước nào tốn nhất? Bước 2-6 (15-20 phút/case)               │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                        │
│ Bước 2-7: LLM phân tích complaint → extract primary issue    │
│ + list subsidiary services involved → recommend routing +    │
│ draft structured case + draft response template              │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ 1. Giảm thời gian xử lý từ 18 phút ──> dưới 3 phút          │
│    (Efficiency).                                             │
│ 2. Tỉ lệ routing đúng team (first-touch) tăng từ 70% ──>    │
│    >= 92% (Accuracy).                                        │
│ 3. Tỉ lệ khách không phải nói lại case tăng từ 55% ──>      │
│    >= 85% (Customer Experience).                             │
│                                                             │
│ Quick Architecture: [x] LLM Reasoning (Multi-step)           │
│                    [ ] Simple Rule (Quá phức tạp)           │
│                    [ ] Agent (Chưa cần orchestration)        │
└─────────────────────────────────────────────────────────────┘
```

---

## QUICK PROBLEM CARD #2: Vinhomes — Phân Loại & Route Khiếu Nại Cư Dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Cư dân gửi khiếu nại/phản ánh qua App Vinhomes   │
│ Resident (ví dụ: "Nước bị mất ở tầng 15 tòa A", "Đèn công  │
│ cộng bị hỏng ở lối vào", "Ồn từ hàng xóm đêm"). Hiện tại    │
│ một nhân viên CSKH phải đọc từng tin và ghi vào biểu mẫu,   │
│ sau đó điều hướng tay sang ban quản lý/kỹ thuật đúng tòa    │
│ nhà (thủ công, mất 15-20 phút/50 tin, delay 12+ giờ).       │
│                                                             │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau?                                                │
│ - Cư dân (chờ lâu để được xử lý, không biết status)         │
│ - CSKH staff (công việc lặp lại cực tải)                    │
│ - Ban quản lý (không biết ưu tiên xử lý cái gì trước)       │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi tin qua App (chứa info khiếu nại tự do)     │
│   → 2. CSKH đọc tin, hiểu vấn đề, phân loại tay (Urgent/   │
│       Normal)                                               │
│   → 3. Ghi vào spreadsheet hoặc hệ thống task, thêm tòa nhà │
│   → 4. Gửi tay/email tới ban quản lý tòa nhà đó             │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (15-20 phút/50 tin)              │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                        │
│ Bước 2-3: LLM phân loại tự động (category: nước/điện/        │
│ ồn/vệ sinh v.v.), extract toạ độ tòa nhà, độ ưu tiên        │
│ → Tạo task được gán tự động sang hệ thống quản lý.          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ 1. Giảm thời gian phân loại & routing từ 15 phút ──> dưới   │
│    30 giây (Efficiency).                                     │
│ 2. Độ chính xác phân loại đạt >= 95% (Accuracy).             │
│ 3. Giảm delay xử lý từ 12+ giờ ──> dưới 2 giờ (SLA).         │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Tự động phân loại)     │
│                    [ ] Rule (Quá nhiều case ngoài lệ)       │
│                    [ ] Agent (Không cần orchestration)      │
└─────────────────────────────────────────────────────────────┘
```

---

## QUICK PROBLEM CARD #3: Vinmec — Soạn Tóm Tắt Hồ Sơ Xuất Viện (Discharge Summary)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Bác sĩ Vinmec khi cho bệnh nhân xuất viện phải    │
│ viết tóm tắt hồ sơ từ bệnh án điện tử (chẩn đoán, xét       │
│ nghiệm, hình ảnh, thuốc kê đơn, hướng dẫn theo dõi sau      │
│ xuất viện). Hiện tại bác sĩ phải soạn thủ công bằng lời    │
│ văn tiếng Việt cho bệnh nhân hiểu được (mất 20-30 phút/      │
│ bệnh nhân, bác sĩ phàn nàn vì quá tải, ảnh hưởng chất lượng│
│ hồ sơ).                                                      │
│                                                             │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau?                                                │
│ - Bác sĩ (tải công việc cao, khó tập trung vào bệnh nhân)   │
│ - Bệnh nhân (hồ sơ chậm, không hiểu rõ hướng dẫn theo dõi)   │
│ - Bộ phận hành chính (phải nhắc nhở, đôi khi bỏ sót)         │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bác sĩ kết thúc khám bệnh, input chẩn đoán + xét       │
│      nghiệm vào hệ thống                                    │
│   → 2. Bác sĩ mở file bệnh án từ database, đọc toàn bộ      │
│      thông tin lâm sàng                                     │
│   → 3. Bác sĩ viết thủ công bản tóm tắt tiếng Việt (Ai      │
│      đang bệnh gì, phải làm gì, tái khám khi nào)           │
│   → 4. In ra hoặc gửi email cho bệnh nhân                   │
│                                                             │
│ Bước nào tốn nhất? Bước 3 (20-30 phút/lượt)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                        │
│ Bước 2.5: LLM tự động draft tóm tắt từ bệnh án → Bác sĩ    │
│ review 2-3 phút rồi approve. Output = tóm tắt dạng tiếng     │
│ Việt dễ hiểu cho bệnh nhân + hướng dẫn theo dõi.            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ 1. Giảm thời gian soạn từ 25 phút ──> dưới 5 phút           │
│    (Efficiency).                                             │
│ 2. Tỉ lệ tóm tắt được chấp nhận (bác sĩ không cần chỉnh      │
│    sửa) đạt >= 80% (Quality).                                │
│ 3. Độ hài lòng bệnh nhân với tính rõ ràng hồ sơ tăng từ      │
│    75% ──> 90% (Patient Satisfaction).                       │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Draft + Bác sĩ review) │
│                    [ ] Agentic Loop (Không cần phức tạp)     │
└─────────────────────────────────────────────────────────────┘
```

---

## Quyết định lựa chọn bài toán

**Nhóm quyết định chọn bài toán #1 (AI Cross-Ecosystem Case Resolver)** để thực hiện Deep-Dive trong Phase 3.

### Lý do lựa chọn và loại bỏ:

| Bài Toán | Lý Do Chọn / Loại Bỏ |
|---------|------------------|
| **#1 — AI Cross-Ecosystem Case Resolver** | CHỌN: Bài toán đa-chiều, thách thức cao (phân tích multi-issue → single structured case), giá trị kinh doanh lớn (giảm rework từ 30% → 8%, tăng first-touch resolution từ 70% → 92%), LLM reasoning showcase tốt (multi-step reasoning, structured output, cross-domain reasoning), Human-in-the-loop vẫn review trước gửi (safe). |
| **#2 — Vinhomes Phân Loại Khiếu Nại** | Loại bỏ: Mặc dù simple và safe nhưng phạm vi quá hẹp (single-domain classification), bài toán #1 có giá trị học tập cao hơn vì yêu cầu multi-step reasoning + cross-system integration. |
| **#3 — Vinmec Discharge Summary** | Loại bỏ: Liên quan y tế → cần compliance cao (HIPAA-like), rủi ro legal nếu draft sai thông tin y khoa. Nên bắt đầu từ use case low-risk trước (như #1). |


