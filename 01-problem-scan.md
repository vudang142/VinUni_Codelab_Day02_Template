# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | **VinFast** | Time-consuming & Stakeholder Pain | Xử lý yêu cầu bảo hành pin/xe: Kỹ thuật viên đối soát thủ công mã lỗi OBD-II, log xe điện và lịch sử bảo dưỡng qua file rời rạc gây chậm trễ giải quyết yêu cầu. |
| 2 | **Xanh SM** | Repetitive & Stakeholder Pain | Giải quyết khiếu nại cước phí phát sinh do đi lệch lộ trình/lỗi GPS: Nhân viên CSKH phải mở bản đồ tra lại từng tọa độ, lịch sử chuyến đi và lý do tài xế cung cấp. |
| 3 | **Vinhomes** | Repetitive & Time-consuming | Phân loại và điều phối ticket sự cố cư dân gửi qua Vinhomes Resident App (tiếng ồn, rò rỉ nước, gửi xe, phí quản lý) tới đúng ban quản lý từng tòa/phân khu. |
| 4 | **Vinmec** | Time-consuming & AI-upgrade | Tổng hợp bệnh sử và tóm tắt hồ sơ bệnh án ngoại trú trước giờ khám cho bác sĩ chuyên khoa nhằm rút ngắn thời gian chuẩn bị và tránh sót tiền sử dị ứng thuốc. |
| 5 | **Vinpearl** | AI-upgrade & Repetitive | Xử lý tư vấn combo đặt phòng, vé VinWonders và điều phối xe đón tiễn: Hiện phụ thuộc telesale đọc bảng chính sách phức tạp, tỷ lệ bỏ rơi cuộc gọi cao vào giờ cao điểm. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #01                                      │
│                                                             │
│ Bài toán (1 câu): Tự động trích xuất mã lỗi xe điện (DTC)   │
│ và đối soát điều kiện bảo hành pin/linh kiện tự động.       │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Kỹ thuật viên xưởng dịch vụ & Chuyên   │
│ viên phê duyệt bảo hành tại trụ sở chính.                   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đọc mã lỗi OBD ──> 2. Tra cứu thủ công điều khoản sổ   │
│   bảo hành ──> 3. Làm báo cáo ticket ──> 4. Chờ duyệt HQ    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 35 phút/xe)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và Bước 3: RAG │
│ tra cứu tài liệu bảo hành theo model xe + sinh bản nháp đề  │
│ xuất phê duyệt bảo hành.                                    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian xử lý 1 hồ sơ bảo hành từ 45 min ──> 8 min;│
│   tăng độ chính xác phân định lỗi bảo hành/lỗi người dùng   │
│   lên >95%.                                                 │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #02                                      │
│                                                             │
│ Bài toán (1 câu): Tự động hóa thẩm định và giải quyết khiếu │
│ nại lệch cuốc/chênh cước phí xe điện bằng phân tích vết GPS.│
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH Xanh SM & Tài xế taxi   │
│ (bị giữ tiền đối soát lâu).                                 │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Tiếp nhận khiếu nại cước ──> 2. Mở log GPS chuyến đi   │
│   ──> 3. So khớp với báo cáo kẹt xe/thời tiết ──> 4. Tính lại│
│   tiền & hoàn trả/phạt                                      │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 15 phút/case)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3: Agent phân│
│ tích spatial trace GPS, đối chiếu lý do đường cấm/kẹt xe    │
│ thời gian thực để ra kết luận hoàn tiền.                    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Tự động hóa 75% số case khiếu nại cước thông thường; giảm │
│   thời gian giải quyết khiếu nại từ 24h ──> under 3 phút.   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #03                                      │
│                                                             │
│ Bài toán (1 câu): Phân loại ngữ nghĩa phản ánh cư dân và tự │
│ động điều phối ticket tới đúng tổ đội kỹ thuật/an ninh.    │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên trực tổng đài ban quản lý tòa│
│ nhà (BQL) & Cư dân bức xúc vì bị chậm trễ.                 │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận text/ảnh phản ánh từ App ──> 2. Đọc & đoán thuộc  │
│   phòng ban nào ──> 3. Tạo task trên hệ thống nội bộ ──>    │
│   4. Gọi bộ đàm/nhắn Zalo cho kỹ thuật viên tòa             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 10 phút/case,│
│ dễ chuyển nhầm đội gây đùn đẩy trách nhiệm)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 & 2: Multimodal│
│ LLM đọc text + ảnh cư dân gửi, gán tag phân loại, trích     │
│ xuất mức độ khẩn cấp và tự động tạo ticket đúng đầu mối.    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Giảm tỷ lệ gán nhầm ban phụ trách từ 18% xuống <2%; giảm  │
│   thời gian tiếp nhận ban đầu từ 15 min ──> under 30 giây.   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘