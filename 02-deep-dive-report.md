# Lab 02 — Deep-Dive Report: AI Cross-Ecosystem Case Resolver (Vin Smart Future)
# Phase 3 — DEEP-DIVE (Nhóm)

## 3.1. Current-State Workflow Mapping

### Quy trình xử lý khiếu nại cross-ecosystem hiện tại:

```
┌──────────────────────┐
│ Bước 1 (2 phút)      │
│ Khách gọi/chat qua   │
│ tổng đài Vin Smart   │
│ (mô tả tự do, lộn xộn│
│ Ai: Khách hàng       │
│ In: Nội dung gốc     │
│ Out: Cuộc gọi ghi âm │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 2 (4 phút) - BOTTLENECK 1   │
│ CSKH đọc nội dung, hiểu vấn đề    │
│ (cần xác định issue primary +     │
│ các issue phụ liên quan)          │
│ Ai: CSKH                          │
│ In: Ghi âm/text mô tả             │
│ Out: Hiểu biết (trong đầu, chưa   │
│ structured)                       │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 3 (3 phút)                  │
│ CSKH tra cứu hóa đơn/đặt phòng/   │
│ chuyến xe liên quan trong các     │
│ hệ thống khác nhau                │
│ Ai: CSKH (tra cứu thủ công)       │
│ In: Issue mô tả                   │
│ Out: Thông tin từ 3-4 hệ thống    │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 4 (4 phút) - BOTTLENECK 2   │
│ CSKH xác định team/bộ phận chịu   │
│ trách nhiệm xử lý                 │
│ (có thể cần phối hợp nhiều team)  │
│ Ai: CSKH (logic suy diễn tay)     │
│ In: Thông tin từ hệ thống         │
│ Out: Routing quyết định (có thể   │
│ sai)                              │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 5 (2 phút)                  │
│ CSKH thu thập thêm context:       │
│ SLA, lịch sử khách, priority      │
│ Ai: CSKH                          │
│ In: Routing quyết định            │
│ Out: Additional context           │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 6 (3 phút) - BOTTLENECK 3   │
│ CSKH viết lại toàn bộ case bằng   │
│ lời văn chuẩn hóa để team xử lý   │
│ hiểu được (structured summary)    │
│ Ai: CSKH (soạn thảo)              │
│ In: Tất cả thông tin trên         │
│ Out: Case summary (text chuẩn)    │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 7 (1 phút)                  │
│ CSKH gửi/routing case sang team   │
│ xử lý                             │
│ Ai: CSKH (click send/assign)      │
│ In: Case summary                  │
│ Out: Case ticket trong hệ thống   │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 8 (1 phút)                  │
│ CSKH theo dõi & nhắc nhở nếu case │
│ không được xử lý trong SLA        │
│ Ai: CSKH                          │
│ Out: Status update                │
└──────────────────────────────────┘

Tổng thời gian: 18-20 phút/case
Tỉ lệ routing đúng (first-touch): 70%
Tỉ lệ khách phải nói lại case: 30%
```

---

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|-------|-------------------|
| **1. Actor / Operator** | Nhân viên CSKH thuộc Trung tâm Xử lý Khiếu Nại Cross-Ecosystem của Vin Smart Future. |
| **2. Current Workflow** | Khi khách hàng gọi/chat để khiếu nại về vấn đề liên quan nhiều dịch vụ Vingroup (VD: Vinpearl phòng sai, VPoint charge, Xanh SM chậm), CSKH phải: (1) đọc nội dung mô tả tự do, (2) suy diễn vấn đề chính, (3) tra cứu thủ công trên 3-4 hệ thống khác nhau, (4) xác định team trách nhiệm, (5) viết lại case bằng cấu trúc chuẩn, (6) gửi sang team xử lý. Mất 18-20 phút/case, high error rate. |
| **3. Bottleneck** | Bước 2-6 (tốn 14 phút): Hiểu & phân tích nội dung không structured + routing logic phức tạp + viết lại case thủ công dễ sai hoặc bỏ sót thông tin. Dẫn đến 30% khách phải nói lại, 20% case routing sai team. |
| **4. Business Impact** | 1. Vin Smart Future xử lý ~200 khiếu nại cross-ecosystem/ngày. Nếu cứ tốn 18 phút, tổng cộng 60 giờ nhân công/ngày. 2. Tỉ lệ rework (khách phàn nàn vì được routing sai team) là 20%, dẫn đến sự hài lòng giảm (~CSAT < 70%). 3. SLA thường miss vì case bị routing sai phải forward lại, tăng time-to-resolution từ 4 giờ → 12 giờ. |
| **5. Success Metric** | 1. Giảm thời gian xử lý case từ 18 phút → dưới 3 phút (Efficiency: -83%). 2. Tỉ lệ routing đúng team (first-touch) tăng từ 70% → >= 92% (Accuracy: +31%). 3. Tỉ lệ khách KHÔNG phải nói lại case tăng từ 70% → >= 85% (Customer Experience). 4. CSAT tăng từ 68% → >= 82%. |
| **6. Operational Boundary** | AI được phép: (1) phân tích & extract vấn đề chính + sub-issues từ nội dung tự do, (2) recommend routing (list candidate teams + confidence score), (3) draft structured case summary (JSON format). TUYỆT ĐỐI KHÔNG được: (1) tự động gửi case mà CSKH chưa review, (2) xác định quyết định final routing (CSKH phải confirm), (3) sửa hay bỏ thông tin của khách (phải keep nguyên). Human-in-the-loop: CSKH luôn xem lại draft trước khi confirm. Fallback: Nếu AI confidence < 60%, yêu cầu CSKH xác nhận manual. |

---

## 3.3. Future-State Flow & AI Fit

### AI Fit Matrix:
- **Chọn:** LLM Reasoning (Multi-step)
- **Tại sao không Rule?** Vấn đề khách mô tả có cấu trúc không cố định, ngôn ngữ tự do, cần suy diễn bước-by-bước → Rule không đủ linh hoạt.
- **Tại sao không Agent?** Quy trình có cấu trúc rõ ràng (input → analyze → route → output), không cần orchestration phức tạp hay agentic loop lặp lại.

### Future-State Flow (với AI):

```
┌──────────────────────┐
│ Bước 1 (2 phút)      │
│ Khách gọi/chat qua   │
│ tổng đài Vin Smart   │
│ (mô tả tự do)        │
│ In: Nội dung gốc     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 2 (0.5 phút) - AI STEP 🔵   │
│ LLM phân tích nội dung:           │
│ - Extract primary issue           │
│ - List subsidiary issues          │
│ - Identify services involved      │
│   (VinPearl? VPoint? Xanh SM?...) │
│ → Output: Structured Issue Card   │
│ (JSON)                            │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 3 (0.5 phút) - AI STEP 🔵   │
│ LLM recommend routing:            │
│ - List candidate teams (top 3)    │
│ - Confidence score mỗi team       │
│ - Tỉ lệ phối hợp (1 team hay >1)  │
│ → Output: Routing Recommendation  │
│ (JSON list với score)             │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 3.5 (0.5 phút) - AI STEP 🔵 │
│ LLM draft case summary:           │
│ - Tóm tắt issue ngắn gọn          │
│ - Customer profile (extract)      │
│ - Impact/urgency classification   │
│ - Recommended actions (draft)     │
│ → Output: Draft Case Summary      │
│ (text + JSON)                     │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 4 (0.5 phút) - HUMAN STEP 🟢│
│ CSKH review AI outputs:           │
│ 1. Xem Issue Card (đúng không?)   │
│ 2. Chọn routing team từ gợi ý    │
│    (hoặc override nếu cần)        │
│ 3. Review case summary            │
│ 4. Add thêm note nếu cần           │
│ HITL Decision: Approve/Reject     │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│ Bước 5 (1 phút)                  │
│ CSKH gửi case với AI-drafted      │
│ summary → Team xử lý              │
│ Out: Case ticket (structured)     │
└──────────────────────────────────┘

Tổng thời gian: ~3-4 phút/case (giảm -80%)
Routing accuracy: >= 92%
Rework rate: < 8%
```

### Ranh giới an toàn (Operational Boundary):

| Quy Tắc | Mô Tả | Implementation |
|---------|-------|-----------------|
| **HITL Mandatory** | AI không được tự gửi case mà không có CSKH xác nhận | Nút "Send" chỉ hoạt động sau khi CSKH click "Confirm" |
| **Confidence Threshold** | Nếu AI routing confidence < 60%, yêu cầu CSKH chọn manual | Logic: if confidence < 0.6, show "Manual Selection Required" badge |
| **Data Integrity** | Không được sửa, xóa, hoặc thêm thông tin từ khách | AI chỉ structured/summarize, không thêm nội dung mới |
| **Fallback** | Nếu LLM bị timeout hay error, fallback về quy trình thủ công cũ | Graceful fallback: show error, offer "Classic Mode" button |
| **Audit Trail** | Ghi log tất cả quyết định của AI (confidence, routing, timestamp) | Log = case_id + ai_decision + cskh_decision + match_score |

---

# Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm)

Nhóm sẽ lập trình bản mẫu prompt bằng **Gemini 2.5 Flash** để kiểm tra xem ranh giới an toàn có vững chắc không.

**File code:** [starter-code/prompt_prototype.py](../starter-code/prompt_prototype.py)

---

# Phase 5 — EVALUATE (Nhóm)

## 5.1. AI Readiness Checklist

| Tiêu Chí | Có? | Lý Giải |
|---------|-----|--------|
| Chúng tôi có sẵn dữ liệu mẫu để test model? | Có | Dữ liệu: 100+ case lịch sử từ Vin Smart Future (anonymized), chứa original complaint + routing decision + outcome. |
| Rủi ro khi AI sai có nằm trong tầm kiểm soát? | Có | Nếu AI routing sai, CSKH vẫn review trước gửi (HITL) → case sẽ được điều chỉnh trước đến team cuối cùng. Worst case: case bị forward lại (+2 giờ delay, không phải +8 giờ như hiện tại). |
| Stakeholders sẵn sàng thay đổi quy trình cũ? | Có | Đã khảo sát team CSKH & team Ops (20 người) → 85% đồng ý thay đổi nếu tool tiết kiệm thời gian >= 70%. |
| Dữ liệu test đã được review để đảm bảo quality? | Có | 100 case đã được data engineer review (remove PII, verify accuracy). Confidence: 95%. |
| Có metric baseline để so sánh? | Có | Current baseline: 18 min/case, 70% accuracy, 68% CSAT. Target: 3 min/case, 92% accuracy, 82% CSAT. |

---

## 5.2. Quyết định Cuối Cùng

### Ban Giám Đốc Vin Smart Future quyết định:

[ ] **GO (Bắt đầu xây dựng Prototype)** ✅ **SELECTED**
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline)**
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn)**

### Justification (Lý Giải Quyết Định):

**Kỹ Thuật:**
1. **LLM Capability Match:** Bài toán yêu cầu multi-step reasoning (understand → analyze → recommend) → Gemini 2.5 Flash đủ khả năng (cost-effective + low latency).
2. **Ranh Giới An Toàn:** HITL (Human-in-the-loop) ở bước 4 giảm rủi ro → worst case là forward lại (+2h), không phải sai hoàn toàn.
3. **Data Ready:** 100+ test cases sẵn sàng (anonymized, reviewed) → có thể bắt đầu fine-tuning/prompt engineering ngay.

**Chi Phí - Lợi Ích:**
1. **Tiết kiệm nhân công:** 200 case/ngày × (18-3) phút = 3,000 phút = 50 giờ nhân công/ngày. Mỗi tháng: ~1,000 giờ × $15/h = $15K chi phí nhân công tiết kiệm.
2. **Tăng CSAT:** Giảm rework từ 30% → 8%, tăng CSAT từ 68% → 82% → khách giữ lại, giảm churn.
3. **Chi phí AI:** Gemini API cost ~$0.075/1M tokens. 200 case/ngày × 300 tokens/case × 20 ngày = 1.2M tokens = $0.09/ngày = ~$2.7/tháng. **ROI: +5000x**.

**Rủi Ro:**
- **Rủi Ro Thấp:** HITL review → không có case nào tự động gửi sai.
- **Rủi Ro Trung Bình:** Nếu AI recommendation sai (< 60% confidence), CSKH vẫn phải manual → không tiết kiệm thời gian cho case đó, nhưng fallback logic sẵn sàng.
- **Mitigation:** Prometheus monitoring + daily accuracy report → detect & fix model drift nhanh.

---

### Kế Hoạch Tiếp Theo (Post GO Decision):

1. **Week 1-2:** Hoàn thiện prompt engineering + test với 100 case → achieve 90%+ accuracy.
2. **Week 3:** Deploy prototype (closed beta với 10% traffic) → monitor real-world performance.
3. **Week 4-5:** Ramp up → 100% traffic (nếu metric pass).
4. **Ongoing:** A/B test (AI-assisted vs manual) → measure CSAT, time-to-resolution, rework rate.

---

## Lưu Ý Cuối Cùng

- **Phạm Vi Bắt Đầu (MVP):** Chỉ handle cross-ecosystem cases (>= 2 services involved). Single-service complaints vẫn dùng quy trình cũ.
- **Data Privacy:** Tất cả khách hàng data phải anonymized trước khi đưa vào prompt (remove names, phone, ID).
- **Escalation Path:** Nếu case quá phức tạp (AI confidence < 40%), tự động escalate sang Senior CSKH thay vì block.

**Hoàn thành Phase 3, 4, 5! Sẵn sàng bước vào Phase 6 — Reflection & AI Log.** 🎉
