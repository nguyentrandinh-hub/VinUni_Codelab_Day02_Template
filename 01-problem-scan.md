| Họ và tên | Mã học viên (MHV) | Email |
| --- | --- | --- |
| Nguyễn Diệu Vy | 2A202603381 | dieuvy2912@gmail.com |

# 01 — Problem Scan & Quick Problem Cards (Cá nhân)

---

# 🔍 Phase 1 — SCAN

Quét qua 4 lenses: Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | **Vinpearl** | Tốn thời gian | Email đặt phòng theo đoàn (Group Booking) từ công ty lữ hành viết tự do. Nhân viên đọc tay, trích ngày/số khách/loại phòng, tra quỹ phòng và soạn báo giá. AI có thể trích yêu cầu và soạn **bản nháp** phản hồi cho nhân viên duyệt. |
| 2 | **VinFast** | Lặp lại | Đối chiếu dữ liệu sạc từ các trạm đối tác với hóa đơn gửi về tài chính hằng tuần. Dữ liệu có cấu trúc, quy tắc khớp rõ ràng nên **rule/script** nhiều khả năng đủ, chưa cần LLM. |
| 3 | **Vinhomes** | AI có thể tốt hơn | Cư dân hỏi thủ tục hành chính (đăng ký thi công nội thất, vé gửi xe tháng). Trợ lý hiện tại trả lời rập khuôn, cư dân vẫn phải gặp ban quản lý để biết cần giấy tờ gì. |
| 4 | **Xanh SM** | Pain từ người khác | Điểm đón khách mô tả bằng tin nhắn tự do ("cổng sau tòa A", "đối diện cây xăng"), GPS lệch; tài xế phải gọi lại khách xác nhận, tốn thời gian và dễ bị hủy chuyến. |
| 5 | **VinUni** | Lặp lại | Giảng viên/trợ giảng chấm lab bằng autograder rồi viết nhận xét sư phạm cho từng sinh viên. Phần nhận xét lặp lại theo mẫu lỗi, LLM có thể soạn nháp để giảng viên chỉnh. |

**Nhận xét nhanh:** #2 là ví dụ nên bắt đầu bằng rule (No-AI/Rule); #1, #3, #5 có đầu vào là ngôn ngữ tự nhiên nên hợp LLM hơn; #4 cần dữ liệu GPS nên LLM chỉ là một phần của giải pháp.

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn #1 (Vinpearl Group Booking), #2 (VinFast đối chiếu sạc), #3 (Vinhomes thủ tục hành chính). Lý do: ba bài toán có ba mức AI-fit khác nhau (LLM / Rule / LLM có rủi ro nội dung pháp lý) nên so sánh được kiến trúc, không chỉ so sánh công ty.

## CARD #1 — Vinpearl: Soạn nháp phản hồi email đặt phòng đoàn

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Trích yêu cầu từ email đặt phòng đoàn và soạn     │
│ NHÁP phản hồi (hoặc câu hỏi bổ sung) cho nhân viên duyệt.   │
│ Công ty thành viên: [x] Khác (Vinpearl)                     │
│                                                             │
│ Ai đang đau? Nhân viên Sales/Reservation (đọc, tra, soạn);  │
│ Revenue/Sales Manager (duyệt giá); agency chờ phản hồi.     │
│                                                             │
│ Workflow thủ công hiện tại (giả định, 5 bước):              │
│  1. Nhận email ──> 2. Đọc & trích yêu cầu ──> 3. Tra quỹ    │
│  phòng ──> 4. Soạn báo giá/thư ──> 5. Quản lý duyệt, gửi    │
│                                                             │
│ Bước tốn nhất? Bước 2 (~8 phút) và 3 (~10 phút) [GIẢ ĐỊNH]  │
│ Tổng thao tác ≈ 30 phút/yêu cầu [GIẢ ĐỊNH, cần đo baseline] │
│ AI hỗ trợ ở bước nào? Bước 2 (trích) và 4 (soạn nháp).      │
│ Bước 3 tra quỹ phòng vẫn do người; AI không có quyền ghi.   │
│                                                             │
│ Metric: Thời gian thao tác/yêu cầu 30 ──> ≤ 18 phút (-40%); │
│ ≥ 95% trường trích xuất đúng trên 30 email gán nhãn;        │
│ 0 vi phạm ranh giới (tự gửi / báo giá / hứa giảm giá).      │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent    │
└─────────────────────────────────────────────────────────────┘
```

- **Rủi ro / ranh giới:** AI không tự gửi, không xác nhận/giữ phòng, không báo giá, không hứa chiết khấu; yêu cầu giảm > 5% chuyển Revenue Manager; email là dữ liệu, không phải lệnh (prompt injection); không lộ thông tin đoàn khác.
- **Giả định cần kiểm chứng:** quy trình thực tế có đúng 5 bước không; email đến qua kênh nào; có quỹ phòng/PMS truy cập được không; ngưỡng 5% là giả định của prototype, chính sách thật do Revenue Manager quyết định.

## CARD #2 — VinFast: Đối chiếu dữ liệu sạc đối tác với hóa đơn

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Khớp bản ghi phiên sạc của trạm đối tác với hóa   │
│ đơn gửi về tài chính, đánh dấu chênh lệch để kế toán xử lý. │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Nhân viên kế toán/đối soát hằng tuần.          │
│                                                             │
│ Workflow thủ công (giả định):                               │
│  1. Tải file phiên sạc ──> 2. Tải hóa đơn ──> 3. Khớp theo  │
│  mã phiên/biển số/kWh ──> 4. Ghi chú chênh lệch ──> 5. Báo  │
│                                                             │
│ Bước tốn nhất? Bước 3 (khớp thủ công trên Excel) [GIẢ ĐỊNH] │
│ AI hỗ trợ ở đâu? Gần như không cần: khớp theo khóa cố định. │
│ LLM chỉ đáng thử để giải thích chênh lệch bất thường.       │
│                                                             │
│ Metric: Thời gian đối soát/tuần ___ ──> ___ (baseline cần   │
│ đo); % bản ghi tự khớp ≥ ___; 0 chênh lệch bị bỏ sót.       │
│                                                             │
│ Quick Architecture: [ ] No AI [x] Rule [ ] LLM [ ] Agent    │
└─────────────────────────────────────────────────────────────┘
```

- **Vì sao Rule:** dữ liệu có cấu trúc, tiêu chí khớp xác định; LLM thêm chi phí và rủi ro "bịa" số liệu tài chính mà không thêm giá trị.

## CARD #3 — Vinhomes: Trợ lý hướng dẫn thủ tục hành chính cho cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Trả lời cư dân cần giấy tờ/bước nào cho thủ tục   │
│ (thi công nội thất, vé gửi xe tháng) và soạn nháp hồ sơ.│
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Cư dân (phải đi lại hỏi); nhân viên ban quản   │
│ lý (trả lời câu hỏi lặp).                                   │
│                                                             │
│ Workflow thủ công (giả định):                               │
│  1. Cư dân hỏi ──> 2. Nhân viên tra quy định ──> 3. Trả lời │
│  danh sách giấy tờ ──> 4. Cư dân nộp ──> 5. BQL kiểm tra    │
│                                                             │
│ Bước tốn nhất? Bước 2–3 lặp lại cho các câu hỏi giống nhau. │
│ AI hỗ trợ ở đâu? Bước 2–3: tìm quy định đã duyệt và soạn    │
│ câu trả lời có trích nguồn; nhân viên duyệt các ca mơ hồ.   │
│                                                             │
│ Metric: % câu hỏi trả lời đúng nguồn quy định ≥ ___ (cần    │
│ tập câu hỏi gán nhãn); thời gian phản hồi ___ ──> ___.      │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent    │
└─────────────────────────────────────────────────────────────┘
```

- **Rủi ro:** quy định/phí sai gây khiếu nại; cần kho quy định đã duyệt và nguồn trích dẫn. Dữ liệu này chưa chắc sẵn có nên thua Card #1 về khả thi.

## So sánh sơ bộ ba card (định tính)

| Tiêu chí | #1 Vinpearl | #2 VinFast | #3 Vinhomes |
|---|---|---|---|
| Độ rõ của pain | Rõ ở mức tác vụ (đọc, tra, soạn); số phút chưa đo | Rõ nhưng có thể đã được tự động hóa một phần | Rõ ở câu hỏi lặp; mức độ chưa biết |
| AI fit | Cao: email tự do → dữ liệu có cấu trúc + nháp | Thấp: Rule đủ | Trung bình–cao, cần RAG |
| Khả thi prototype | Cao: tự tạo email giả lập, không cần dữ liệu thật | Cao nhưng kém giá trị AI | Trung bình: cần kho quy định đã duyệt |
| An toàn | Trung bình: sai giá/hứa phòng gây thiệt hại → bắt buộc HITL | Trung bình: sai số liệu tài chính | Trung bình–cao: sai quy định/phí |
| Đo được | Thời gian, % trường đúng, số vi phạm ranh giới | Thời gian, % tự khớp | % đúng nguồn, thời gian phản hồi |

**Stress-test (đóng vai CFO/Trưởng vận hành) cho Card #1:**
1. *Rule-based có làm được không?* Nếu agency dùng form chuẩn thì rule/form đủ; LLM chỉ đáng khi email là văn bản tự do. → Cần đo tỷ lệ email tự do/có cấu trúc.
2. *Tiết kiệm có thật không?* Bước tốn thời gian nhất có thể là tra quỹ phòng (bước 3), AI không chạm vào. → Metric được viết cho phần AI thực sự đụng tới (bước 2, 4).
3. *Rủi ro báo giá sai?* → Ranh giới cứng: AI không báo giá, không xác nhận; người duyệt và gửi.

## 🗳️ Bài toán được chọn để Deep-Dive

**Card #1 — Vinpearl Group Booking.** Lý do: (1) đầu vào là ngôn ngữ tự do nên LLM có giá trị thật; (2) dùng được dữ liệu hoàn toàn giả lập, không đụng thông tin khách thật; (3) có ranh giới vận hành rõ để stress-test (không tự gửi, không báo giá, giảm giá > 5% phải leo thang); (4) khả thi hơn Card #3 vì không phụ thuộc kho quy định; (5) Card #2 nên giải bằng Rule.
