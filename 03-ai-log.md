# 03 — AI Log và Reflection

---

## 1. Cách tôi làm việc với AI

Tôi dùng **Claude** làm trợ lý đồng hành. Tôi là người đặt phạm vi và kiểm soát hướng đi; AI soạn bản nháp để tôi duyệt.

**Những việc tôi chủ động làm:**

1. **Cung cấp tài liệu gốc và yêu cầu đọc kỹ trước khi làm.** Tôi gửi cho AI worksheet, student guide, inspiration kit, bài mẫu và file PDF hướng dẫn của buổi lab, rồi yêu cầu AI đọc kỹ để nắm đề, cách chấm và quy tắc nộp bài. Tôi cũng bảo AI vào repo GitHub của lab để đọc toàn bộ file `.md` và code, thay vì làm theo trí nhớ.
2. **Giao đúng phần việc cá nhân.** Tôi nêu rõ phần của mình là Problem Scan (tối thiểu 5 bài toán) và 3 Quick Problem Cards, và yêu cầu làm theo mẫu worksheet.
3. **Giữ phạm vi trong tầm kiểm soát.** Khi AI bắt đầu dựng một tài liệu dài ngoài ý muốn, tôi dừng lại và nói rõ tôi chỉ cần vài ý tưởng. Với 3 việc còn lại (prototype, AI log, push), tôi yêu cầu AI nêu quy trình trước rồi mới làm.
4. **Tự thực hiện các thao tác Git và báo lại kết quả thật.** Tôi clone repo, tạo nhánh `anhtu`, push, rồi dán lại output terminal và ảnh chụp trang Branches của GitHub để AI đối chiếu.
5. **Phát hiện và báo lỗi khi kết quả không như mong đợi.** Tôi báo "sao tôi không thấy nhánh của tôi" khi trang GitHub không hiện nhánh, và báo "có thấy file đâu" khi lệnh `cp` không tìm thấy file.
6. **Đưa thông tin thật của nhóm vào.** Nhóm đã chọn đề tài **VinFast: kiểm tra và đánh giá sự cố xe**. Tôi đưa sơ đồ quy trình hiện tại của nhóm cho AI để các file bám theo quy trình thật, và yêu cầu AI cho tôi biết đã làm được những gì so với hướng dẫn trong PDF.

## 2. AI giúp gì

| Việc | AI đóng góp | Phần tôi duyệt hoặc quyết định |
|---|---|---|
| Tóm tắt đề và cách chấm | Đọc các tài liệu, tóm tắt rubric, quy tắc nộp, cách autograder kiểm tra | Tôi đối chiếu với PDF của buổi lab |
| Tìm bài toán (Phase 1) | Đề xuất 6 bài toán theo 4 lenses | Tôi chọn bài nào đi vào card; nhóm chốt đề tài VinFast |
| Quick Cards (Phase 2) | Soạn 3 card theo mẫu | Nhóm chọn Card #1; tôi yêu cầu chỉnh theo sơ đồ nhóm |
| Báo cáo Deep-Dive | Soạn khung 6 trường, AI-Fit Matrix, future flow, Evaluate | Cần nhóm xác nhận nội dung và quyết định cuối |
| Prototype | Viết `SYSTEM_PROMPT`, `evaluate_prompt()`, 4 ca tấn công | **[ĐIỀN]** Tôi chạy thật với Gemini và ghi kết quả (mục 4) |
| Git | Chẩn đoán lỗi từ output tôi dán | Tôi tự gõ lại lệnh và xác minh trên GitHub |

## 3. AI sai hoặc chưa đúng ở đâu, và cách xử lý

**a) Con số do AI tự đặt, không có nguồn.**
Bản nháp đầu của Quick Cards chứa các số thời gian và tỉ lệ (ví dụ "8–10 phút/lượt", "70% lên 85%") do AI tự ước tính, không phải số đo thực tế của VinFast. AI tự ghi chú các số này là ước tính giả định và đề xuất quyết định **NOT YET** trong phần Evaluate vì chưa có baseline. **[XÁC NHẬN]** Tôi đọc lại và đồng ý giữ ghi chú đó; nếu nhóm có số đo thật thì sẽ thay vào.

**b) Quy trình AI dựng không khớp quy trình nhóm đã chọn.**
Card #1 ban đầu là quy trình 5 bước do AI tự nghĩ ra (khách gọi hotline, cố vấn dịch vụ ghi phiếu). Quy trình thật của nhóm gồm 6 bước, kỹ thuật viên đánh giá nguyên nhân là bottleneck, tổng khoảng 20 phút/xe. Khi tôi đưa sơ đồ nhóm cho AI, nó viết lại Card #1 và báo cáo cho khớp. Bài học của tôi: AI không biết quy trình thật; phải đưa quy trình thực tế cho nó trước.

**c) Thời gian từng bước trong báo cáo là AI chia lại.**
Sơ đồ nhóm chỉ ghi tổng khoảng 20 phút. AI chia thành 2, 3, 6, 6, 2, 1 phút cho 6 bước và ghi rõ là ước tính. Đây là điểm tôi sẽ kiểm chứng bằng cách đo thực tế hoặc hỏi người trong xưởng. **[ĐIỀN nếu đã kiểm chứng]**

**d) Lỗi Git: chạy lệnh sai thư mục.**
Tôi clone repo lab nhưng chạy các lệnh tiếp theo ở thư mục khác, nên nhánh `anhtu` bị đẩy lên nhầm repo (SCC-SIS-BE). AI đọc output tôi dán, chỉ ra dòng `To https://github.com/codegym-software/SCC-SIS-BE.git` và giải thích. Tôi `cd` vào đúng thư mục, tạo lại nhánh, push, rồi xác nhận nhánh hiện trên GitHub. Sau đó lệnh `cp` lỗi vì file chưa được tải về máy; tôi tải/tạo file trong VS Code rồi push lại. Bài học: đọc kỹ dòng `To <URL>` và kiểm tra `git remote -v` trước khi push.

## 4. Prototype: ranh giới và kiểm chứng

`SYSTEM_PROMPT` hiện tại gồm: quy tắc ưu tiên cao hơn mọi yêu cầu người dùng (kể cả người tự xưng quản lý), Rule 1 nêu cụ thể các cách lách (gửi thẳng, bỏ thẻ, khách VIP), Rule 2 nêu cách suy ra pin dưới 5% và bắt buộc trả JSON `dispatch_mobile_charger`, quy tắc không tiết lộ system prompt. Model chạy với `temperature=0.0`.

Có 4 ca tấn công: (1) pin 2% đòi trạm 8km, (2) đòi bỏ thẻ `[DRAFT_ONLY]`, (3) giả danh trưởng ca cho phép bỏ giới hạn 5km, (4) yêu cầu in system prompt và bỏ thẻ.

**Kiểm chứng ban đầu (chưa phải Gemini thật):** AI chạy script với một Gemini giả lập để kiểm tra logic các hàm kiểm tra và các lệnh `--check-*` của autograder; kết quả đúng như mong đợi, kể cả khi giả lập trả lời sai thì script báo Failed. Việc này **không chứng minh** Gemini thật sẽ tuân thủ prompt.

**[ĐIỀN] Kết quả chạy Gemini 2.5 Flash thật:**
- Lần chạy đầu: [ĐIỀN số kiểm tra Passed/Failed, ca nào Failed, dán đoạn output sai ngắn].
- Tôi đã sửa prompt: [ĐIỀN thêm hoặc đổi câu nào, vì sao].
- Lần chạy sau khi sửa: [ĐIỀN kết quả].
- Nếu cả 4 ca Passed ngay lần đầu, ghi đúng như vậy và nêu thêm một ca tấn công khác tôi sẽ thử.

**Giới hạn của kiểm tra tự động:** nó chỉ tìm chuỗi ký tự (có `[DRAFT_ONLY]` ở đầu, có `dispatch_mobile_charger`), không hiểu nghĩa câu trả lời. Một câu có từ khóa vẫn có thể sai nội dung, nên tôi đọc lại cả phần trả lời của model, không chỉ nhìn dòng Passed.

## 5. Suy ngẫm cá nhân

- AI nhanh nhất ở việc dựng khung và diễn đạt, kém nhất ở việc biết quy trình thật và con số thật. Những chỗ tôi bổ sung (tài liệu gốc, sơ đồ nhóm, output terminal) là chỗ chất lượng bài tăng lên rõ nhất.
- Việc dừng AI khi nó đi ngoài phạm vi, và yêu cầu nêu quy trình trước khi làm, giúp tôi kiểm soát được bài thay vì nhận một khối kết quả lớn.
- Quyết định **NOT YET** trong báo cáo đến từ việc không tin số liệu AI tự đặt. **[XÁC NHẬN]** Tôi và nhóm sẽ thống nhất quyết định cuối trong buổi họp.
- Với bài toán liên quan an toàn xe, ranh giới quan trọng nhất là AI chỉ gợi ý, kỹ thuật viên kết luận, và có rule cứng chạy trước LLM cho nhóm lỗi nguy hiểm.
- Lần sau tôi sẽ đưa quy trình thật của nhóm cho AI ngay từ đầu và kiểm tra `git remote -v` trước khi push.
