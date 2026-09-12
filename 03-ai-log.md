# 03 — AI Log & Reflection

> Deliverable cá nhân — Phase 6 (REFLECTION)
> Người thực hiện: DamVietHung-02600

---

## 1. AI đã giúp gì trong quá trình làm bài?

- **Brainstorm Phase 1 (SCAN):** Dùng AI để mở rộng danh sách bài toán ban đầu — thay vì tự nghĩ ra 5 ý tưởng rời rạc, AI giúp gợi ý theo đúng 4 lenses và gắn thêm ước tính con số (thời gian, tần suất) làm điểm khởi đầu để mình kiểm chứng lại.
- **Stress-test Quick Problem Card:** Dán thẻ bài toán vào AI với vai "CFO/Trưởng phòng Vận hành khắt khe" giúp phát hiện ra Card #2 (Vinhomes) thực ra có thể giải quyết bằng rule-based router đơn giản, không cần LLM — điều mà ban đầu nhóm không nhận ra vì mặc định "cứ có xử lý ngôn ngữ tự nhiên là cần AI".
- **Viết Problem Statement & Future-State Flow:** AI giúp cấu trúc lại 6-field cho mạch lạc và gợi ý cách phân biệt rõ giữa bước AI xử lý (🔵) và bước con người phê duyệt (🟢), đặc biệt hữu ích khi mình lần đầu thiết kế Operational Boundary cho một bài toán y tế.

## 2. AI trả lời sai / hallucination ở đâu?

- Khi hỏi về số liệu vận hành thực tế của Vinmec (số ca xuất viện/ngày, thời gian trung bình soạn tóm tắt), AI đưa ra các con số **nghe rất "chắc chắn" nhưng thực chất là ước lượng chung chung**, không dựa trên dữ liệu thật của Vinmec. Ban đầu mình suýt dùng thẳng những con số này như dữ kiện thật.
- Ở bước đề xuất kiến trúc AI, AI có xu hướng **mặc định gợi ý "Agentic Loop"** cho hầu hết bài toán vì nghe có vẻ "hiện đại" hơn, kể cả với bài toán có quy trình cố định như soạn tóm tắt xuất viện — nơi mà một LLM Feature đơn giản với HITL là đủ và an toàn hơn nhiều.
- Khi được yêu cầu viết Operational Boundary, AI ban đầu bỏ sót phần "AI tuyệt đối không được tự đưa ra chẩn đoán mới" — chỉ tập trung vào việc AI không được gửi tự động, mà quên mất rủi ro AI có thể "tự sáng tạo" thêm nội dung y khoa không có trong hồ sơ gốc (một dạng hallucination nguy hiểm nếu xảy ra trong sản phẩm thật).

## 3. Mình đã sửa prompt / ranh giới ra sao để đạt kết quả chuẩn?

- Với vấn đề số liệu: mình đổi prompt từ "cho tôi số liệu về..." sang **"đây là số liệu tôi tự ước tính dựa trên quy mô Vinmec, hãy giúp tôi kiểm tra tính hợp lý của con số này"** — buộc AI đóng vai phản biện thay vì tự bịa số, và mình luôn gắn nhãn rõ trong báo cáo rằng các con số là *ước tính minh họa cho mục đích bài tập*, không phải dữ liệu thật của Vinmec.
- Với vấn đề mặc định chọn Agent: mình thêm ràng buộc vào prompt — **"chỉ đề xuất Agentic Loop nếu quy trình có nhiều bước ra quyết định động và không thể xác định trước; nếu quy trình cố định, hãy nói rõ vì sao Rule hoặc LLM Feature đủ dùng"** — nhờ đó AI đưa ra lập luận rõ ràng hơn thay vì mặc định chọn giải pháp "kêu" nhất.
- Với Operational Boundary: mình yêu cầu AI **liệt kê riêng 2 nhóm ranh giới** — "AI không được hành động gì" và "AI không được tạo ra nội dung gì" — tách biệt rủi ro về hành vi (action) và rủi ro về nội dung (content), nhờ đó phát hiện ra thiếu sót về việc AI có thể tự thêm chẩn đoán không có thật.

## 4. Bài học rút ra

AI là một thought-partner tốt để mở rộng ý tưởng và đóng vai phản biện, nhưng **không nên tin tưởng con số hoặc kiến trúc đề xuất mặc định** — luôn cần tự đặt câu hỏi ngược lại và yêu cầu AI giải thích lý do, đặc biệt với các bài toán có rủi ro cao như y tế. Việc ép AI tách nhỏ vấn đề (behavior vs. content risk) giúp tư duy Operational Boundary chặt chẽ hơn nhiều so với việc hỏi chung chung "ranh giới an toàn là gì".
