# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Sơ đồ quy trình hiện tại:**
[Khách mang xe đến]
│
▼
[Bước 1: Tiếp nhận & Cắm máy chẩn đoán quét mã DTC] (⏱ 5-10 min)
│
▼ 🔄 Handoff 1 (Kỹ thuật viên xuất log text/PDF gửi lên máy tính bàn)
[Bước 2: 🔴 Tra cứu sổ tay kỹ thuật & Điều khoản bảo hành] (⏱ 20-25 min)

KTV mở file PDF dịch vụ dày hàng trăm trang

Tra cứu thủ công mã lỗi (ví dụ: BMS, Inverter, OBC)

So khớp điều kiện: xe đã bảo dưỡng đúng hạn không, có độ chế linh kiện không
│
▼ 🔴 Bottleneck 2 & 🔄 Handoff 2 (KTV soạn hồ sơ gửi sang Trưởng bộ phận dịch vụ duyệt sơ bộ)
[Bước 3: Soạn tờ trình yêu cầu bảo hành lên phần mềm DMS] (⏱ 15 min)

Nhập liệu thủ công mã phụ tùng, mô tả lỗi, đính kèm file log
│
▼ 🔄 Handoff 3 (Hồ sơ đẩy về HQ - Trung tâm Phê duyệt Bảo hành VinFast Hải Phòng)
[Bước 4: Chuyên viên HQ rà soát và ra quyết định duyệt/từ chối] (⏱ 24 - 48 giờ)
│
▼
[Bước 5: Xuất kho phụ tùng thay thế & Bàn giao xe]

* 🔴 **Bottleneck chính:** 
  * Bước 2 (Tra cứu chéo quy định bảo hành theo model & phiên bản firmware: dễ sót điều kiện loại trừ, tốn thời gian đọc sổ tay).
  * Bước 3 (Nhập liệu thủ công mô tả kỹ thuật từ file log thô, dễ sai lệch thuật ngữ gây trả về làm lại).
* 🔄 **Handoff:** 3 điểm chuyển giao thông tin (KTV ra máy tính -> Trưởng xưởng xưởng dịch vụ -> Trung tâm bảo hành HQ).
* **Tổng thời gian vận hành trung bình:** **~50 phút/lượt xử lý tại xưởng** (chưa tính thời gian chờ HQ phê duyệt 1-2 ngày).

---

## 3.2. Problem Statement (6-field) & Metrics (15 min)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Kỹ thuật viên (KTV) tại Xưởng dịch vụ VinFast 3S và Chuyên viên kiểm duyệt bảo hành tại Trụ sở chính (HQ Warranty Approver). |
| **2. Current Workflow** | KTV cắm máy đọc cổng OBD trích xuất log lỗi DTC $\rightarrow$ Mở sổ tay kỹ thuật & file quy định bảo hành PDF tra cứu thủ công $\rightarrow$ Gõ tay phiếu yêu cầu bảo hành trên hệ thống DMS $\rightarrow$ Chờ HQ duyệt thủ công. |
| **3. Bottleneck** | Bước tra cứu quy định bảo hành đa tầng (mã linh kiện, phiên bản firmware xe, lịch sử bảo dưỡng xe có hợp lệ hay không) và tổng hợp giải trình kỹ thuật tốn nhiều công sức, dễ nhầm lẫn giữa lỗi người dùng và lỗi phần cứng của nhà sản xuất. |
| **4. Business Impact** | Xe nằm chờ xưởng dịch vụ lâu (turnaround time cao) gây nghẽn khoang sửa chữa, chỉ số hài lòng khách hàng (CSAT) sụt giảm, nguy cơ duyệt sai gây rò rỉ chi phí bảo hành hãng hàng tỷ đồng mỗi quý do bỏ sót các trường hợp can thiệp phần cứng trái phép. |
| **5. Success Metric** | **Giảm 80% thời gian tạo hồ sơ bảo hành:** Từ 45-50 phút xuống còn dưới 8 phút/xe. **Tỷ lệ phê duyệt sơ bộ chính xác (First-Time Right Rate):** Đạt $\ge 92\%$ mà không bị HQ trả về do thiếu dữ liệu hoặc sai lệch điều khoản. |
| **6. Operational Boundary** | **Được làm:** Đọc log DTC/CAN-bus, trích xuất mã lỗi, dùng RAG truy vấn sổ tay kỹ thuật, soạn sẵn tờ trình nháp (draft claim), tính toán điểm rủi ro (risk score).<br>**TUYỆT ĐỐI KHÔNG được làm:** Tự động kích hoạt xuất kho phụ tùng thay thế hoặc tự động gửi quyết định từ chối chính thức tới khách hàng.<br>**Điểm cần duyệt:** KTV xác nhận tình trạng thực tế trên xe; Chuyên viên HQ bấm phê duyệt giải ngân bảo hành cuối cùng. |

---

## 3.3. Future-State Flow & AI Fit (25 min)

* **Xác định mức AI Fit (AI-Fit Matrix):**  
  `[ ] Rule / State-Machine`  
  `[ ] LLM Feature`  
  `[x] Agentic Loop` *(Kết hợp deterministic parser để bóc tách log định dạng hex/DTC + RAG Agent tra cứu tài liệu bảo hành theo model + LLM sinh tờ trình và đề xuất quyết định)*.

* **Sơ đồ Future-State Flow:**
[KTV cắm thiết bị đọc log xe điện]│▼[Log file (.json/.txt/hex dump) tự động đẩy lên hệ thống DMS]│▼🔵 [AI Step 1: Parser & DTC Extractor]Bóc tách mã lỗi chuẩn (SAE/ISO DTC) và trạng thái đóng mở relay/nhiệt độ pack pin│▼🔵 [AI Step 2: Warranty RAG Agent]Truy vấn vector DB (Sổ bảo hành VinFast, Bulletin kỹ thuật, Lịch sử bảo dưỡng của VIN xe)Phân tích điều kiện loại trừ (ví dụ: đã bypass phần mềm, sạc quá dòng)Sinh bản thảo hồ sơ đề xuất (Draft Claim Proposal) + Gắn nhãn khuyến nghị (Approve / Reject / Inspect)│▼ (Check confidence score: Confidence $\ge 85\%$)├─────────────────────────────────────────────┐│ (Tự tin cao)                                │ (Confidence < 85% hoặc Case pin đặc thù)▼                                             ▼🟢 [Human Step (HITL) 1: KTV Review]         ↩️ [Fallback: Handoff sang KTV chuyên trách]KTV kiểm tra lại ảnh chụp thực tế        * Chuyển toàn bộ raw log sang choXác nhận tính chính xác (1 click duyệt)    Kỹ sư pin bậc cao rà soát thủ công│                                             │▼                                             ▼🟢 [Human Step (HITL) 2: HQ Approver] ◄──────────────┘Chuyên viên HQ xem màn hình tổng hợp đối chiếuPhê duyệt giải ngân bảo hành (1-click approval)

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?** *(Hệ thống chẩn đoán của VinFast và kho log bảo hành tại xưởng đã lưu trữ theo từng số khung VIN).*
2. [x] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?** *(Có, AI chỉ đóng vai trò Co-pilot chuẩn bị hồ sơ và gợi ý; quyền quyết định xuất linh kiện hoàn toàn thuộc về con người).*
3. [x] **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?** *(KTV và xưởng dịch vụ đang chịu áp lực lớn về KPI thời gian trả xe, rất mong muốn giảm tải việc gõ giấy tờ thủ công).*

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.  
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.  
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> **1. Tính khả thi kỹ thuật & Ranh giới rõ ràng:** Bài toán không thuần túy dựa vào LLM sáng tạo mà là sự kết hợp chặt chẽ giữa code bóc tách dữ liệu có cấu trúc (DTC log parsing) và RAG (Retrieval-Augmented Generation) trên kho tài liệu kỹ thuật đóng của VinFast. Điều này triệt tiêu hiện tượng ảo giác (hallucination) của mô hình.
>
> **2. Tác động kinh tế & ROI nhanh chóng:** Với số lượng xe điện VinFast lưu hành tăng trưởng cấp số nhân (VF 3, VF 5, VF e34, VF 8...), tắc nghẽn tại xưởng dịch vụ sẽ trở thành điểm nghẽn nghiêm trọng ảnh hưởng đến uy tín thương hiệu. Giảm thời gian xử lý thủ tục từ 45 phút xuống dưới 8 phút sẽ trực tiếp nâng công suất phục vụ của các trạm dịch vụ lên ít nhất 25-30% mà không cần tuyển thêm nhân sự back-office.
>
> **3. Scope thử nghiệm an toàn:** Giai đoạn Prototype (PoC) trong 6 tuần chỉ cần giới hạn trên 1 dòng xe phổ biến nhất (ví dụ: VF 5 hoặc VF 8) với nhóm lỗi phổ biến về Pin/BMS và Hệ thống Điều hòa/Thermal management trước khi mở rộng quy mô cho toàn bộ danh mục xe.

---