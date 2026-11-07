# 01 — Problem Scan & Quick Problem Cards (Cá nhân)

**Vai trò:** AI Product Engineer, Vin Smart Future (Vingroup)
**Lưu ý về số liệu:** Các con số thời gian và chỉ số dưới đây là **ước tính giả định** để scoping, chưa phải số đo thực tế. Cần xác minh với đơn vị vận hành trước khi đưa vào kế hoạch triển khai.

---

# 🔍 Phase 1 — SCAN

Quét qua 4 lenses: Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** | AI có thể tốt hơn | Khách mô tả lỗi xe bằng tiếng Việt đời thường ("đi qua gờ giảm tốc kêu cụp cụp ở bánh trước"). Cố vấn dịch vụ phải hỏi lại nhiều lần mới phân loại được hạng mục lỗi để đặt lịch xưởng. |
| 2 | **Vinpearl** | Tốn thời gian | Email đặt phòng theo đoàn (Group Booking) từ công ty lữ hành viết tự do, nhiều yêu cầu lẫn nhau. Nhân viên đọc tay, tra quỹ phòng từng ngày và soạn lệnh book bằng tay. |
| 3 | **Vinhomes** | Lặp lại | Phản ánh của cư dân trên app Vinhomes Resident (mất nước, hỏng đèn, ồn ào...) được CSKH phân loại và chuyển tay đến ban quản lý từng tòa, thường chậm và hay chuyển nhầm. |
| 4 | **Vinmec** | Pain từ người khác | Bác sĩ phải tự viết tóm tắt hồ sơ xuất viện từ bệnh án, xét nghiệm và ghi chú. Việc này lặp lại cho mỗi bệnh nhân và là điểm bác sĩ hay phàn nàn vì quá tải giấy tờ. |
| 5 | **Xanh SM** | Pain từ người khác | Tài xế phàn nàn điểm đón khách gợi ý chưa chính xác (khách nhắn "cổng sau tòa A", GPS lệch). Tài xế mất thời gian gọi lại khách, khách dễ hủy chuyến. |
| 6 | **VinFast** | Lặp lại | Đối chiếu dữ liệu sạc điện từ các trạm đối tác với hóa đơn gửi về tài chính hằng tuần. Việc khớp số liệu lặp đi lặp lại và phần lớn có thể xử lý bằng rule. |

**Nhận xét nhanh:** Bài #6 là ví dụ nên bắt đầu bằng rule-based, chưa cần AI. Bài #1, #2, #3 có đầu vào là ngôn ngữ tự nhiên, phù hợp hơn với LLM.

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 từ bảng SCAN: **#1 (VinFast chẩn đoán lỗi), #2 (Vinpearl Group Booking), #3 (Vinhomes phản ánh cư dân).**

Ba bài này khác nhau về kiểu kiến trúc (LLM, LLM kết hợp rule, rule kết hợp LLM) và mức rủi ro, nên dễ so sánh khi nhóm thảo luận.

## Card #1 — VinFast: Phân loại lỗi xe từ mô tả của khách

```text
QUICK PROBLEM CARD #1

Bài toán (1 câu): Khách mô tả lỗi xe bằng tiếng Việt đời thường, cần
tự động phân loại thành hạng mục kỹ thuật sơ bộ để đặt lịch xưởng đúng.

Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes
                    [ ] Vinmec   [ ] Khác

Ai đang đau (Actor)?
  - Cố vấn dịch vụ (phải hỏi lại nhiều lần, ghi tay)
  - Khách hàng (chờ lâu, mô tả xong vẫn bị hỏi lại)
  - Kỹ thuật viên (nhận phiếu mơ hồ, mất thời gian kiểm tra lại)

Workflow thủ công hiện tại (5 bước):
  1. Khách gọi hotline/nhắn tin mô tả lỗi
  -> 2. Cố vấn dịch vụ hỏi thêm triệu chứng (vị trí, khi nào, âm thanh)
  -> 3. Cố vấn ghi phiếu tiếp nhận, chọn hạng mục lỗi theo kinh nghiệm
  -> 4. Đặt lịch xưởng và chuẩn bị phụ tùng theo phán đoán
  -> 5. Kỹ thuật viên kiểm tra thực tế, sửa lại phân loại nếu sai

Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (ước tính 8-10 phút/lượt,
và phân loại sai kéo theo chuẩn bị nhầm phụ tùng)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: đọc mô tả, gợi ý
3 hạng mục lỗi khả dĩ kèm câu hỏi làm rõ cho cố vấn dùng ngay.

Đo thành công bằng gì (Metric có số)?
  - Giảm thời gian tiếp nhận từ ~10 phút xuống dưới 4 phút/lượt
  - Tỉ lệ phân loại sơ bộ đúng (kỹ thuật viên xác nhận) từ ~70% lên 85%

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
  LLM chỉ gợi ý phân loại. Cố vấn dịch vụ luôn là người xác nhận.
```

**Ranh giới an toàn:** Nếu mô tả có dấu hiệu liên quan phanh, lái, pin cao áp, túi khí hoặc cháy, hệ thống không để AI kết luận mà chuyển ngay cho kỹ sư và nhắc khách dừng xe an toàn. Đây là rule cứng chạy trước LLM.

**Stress-test (CFO / Vận hành):**
- Rule theo từ khóa có đủ không? Với mô tả đời thường rất đa dạng thì khó, nhưng cần chạy thử trên phiếu cũ để so sánh với LLM trước khi chọn.
- Chỉ số "đúng 85%" cần dữ liệu phiếu tiếp nhận cũ để làm baseline, hiện chưa có.
- Chi phí mỗi lượt gọi LLM cần nhỏ hơn giá trị thời gian cố vấn tiết kiệm được.

---

## Card #2 — Vinpearl: Xử lý email Group Booking

```text
QUICK PROBLEM CARD #2

Bài toán (1 câu): Đọc email đặt phòng theo đoàn từ công ty lữ hành,
trích xuất yêu cầu, kiểm tra quỹ phòng và soạn nháp lệnh book.

Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes
                    [ ] Vinmec   [x] Khác: Vinpearl

Ai đang đau (Actor)?
  - Nhân viên đặt phòng/sales Vinpearl (đọc tay, nhập tay)
  - Công ty lữ hành (chờ báo giá và xác nhận lâu)

Workflow thủ công hiện tại (5 bước):
  1. Nhận email yêu cầu đoàn (số khách, ngày, loại phòng, yêu cầu riêng)
  -> 2. Nhân viên đọc và ghi lại yêu cầu vào bảng
  -> 3. Tra quỹ phòng từng ngày trên hệ thống quản lý phòng
  -> 4. Soạn email báo giá/xác nhận và lệnh book
  -> 5. Quản lý duyệt, gửi lại cho công ty lữ hành

Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (ước tính 25-30 phút/đoàn,
dễ sót yêu cầu đặc biệt hoặc nhầm ngày)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và 4: trích xuất yêu cầu
thành bảng có cấu trúc, soạn nháp phản hồi. Bước 3 do rule/API tra
phòng, không để LLM tự "đoán" phòng trống.

Đo thành công bằng gì (Metric có số)?
  - Giảm thời gian xử lý một email đoàn từ ~30 phút xuống dưới 10 phút
  - Giảm tỉ lệ sót hoặc sai yêu cầu (phát hiện sau khi gửi) từ ~8% xuống dưới 2%

Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent
  LLM trích xuất và soạn nháp, rule/API kiểm tra quỹ phòng, nhân
  viên duyệt trước khi gửi.
```

**Ranh giới an toàn:** AI không được tự gửi email hoặc giữ phòng. Mọi lệnh book phải qua nhân viên duyệt. Số phòng trống chỉ lấy từ hệ thống quản lý phòng, không do LLM tự tạo.

**Stress-test (CFO / Vận hành):**
- Nếu mẫu email lữ hành khá cố định thì parser rule/regex có thể đủ, cần khảo sát độ đa dạng của email trước.
- Rủi ro lớn nhất là LLM trích sai ngày hoặc số phòng. Cần bước đối chiếu lại thông tin đã trích với email gốc cho nhân viên thấy.

---

## Card #3 — Vinhomes: Phân loại và điều hướng phản ánh cư dân

```text
QUICK PROBLEM CARD #3

Bài toán (1 câu): Tự động phân loại phản ánh của cư dân trên app
Vinhomes Resident và chuyển đúng ban quản lý, đúng mức ưu tiên.

Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes
                    [ ] Vinmec   [ ] Khác

Ai đang đau (Actor)?
  - Cư dân (phản ánh xong phải chờ lâu, đôi khi bị chuyển nhầm nơi)
  - Nhân viên CSKH (phân loại lặp đi lặp lại cả ngày)
  - Ban quản lý tòa (nhận phản ánh không thuộc mình)

Workflow thủ công hiện tại (4 bước):
  1. Cư dân gửi phản ánh qua app (văn bản, kèm ảnh)
  -> 2. CSKH đọc và chọn nhóm vấn đề (nước, điện, ồn, vệ sinh...)
  -> 3. CSKH chuyển cho ban quản lý đúng tòa, đúng bộ phận
  -> 4. Ban quản lý xử lý và cập nhật trạng thái

Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (ước tính 2-3 giờ từ lúc
gửi đến lúc đúng bộ phận nhận, nhất là ngoài giờ hành chính; chuyển
nhầm khoảng 10-15% sự cố)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: phân loại nhóm vấn đề,
gắn mức ưu tiên và gợi ý bộ phận nhận.

Đo thành công bằng gì (Metric có số)?
  - Giảm thời gian từ lúc gửi đến lúc đúng bộ phận nhận từ ~2-3 giờ
    xuống dưới 15 phút
  - Giảm tỉ lệ chuyển nhầm bộ phận từ ~12% xuống dưới 4%

Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent
  Rule xử lý các loại rõ ràng và quy tắc ưu tiên khẩn cấp, LLM
  xử lý phản ánh mơ hồ, CSKH xem lại các ca độ tin cậy thấp.
```

**Ranh giới an toàn:** Các phản ánh liên quan an toàn (cháy, rò rỉ gas, kẹt thang máy, đe dọa người) xử lý bằng rule khẩn cấp, chuyển trực tiếp đến bộ phận trực, không chờ LLM. AI không trả lời cư dân về phí, hợp đồng, tranh chấp; những nội dung này giao cho CSKH.

**Stress-test (CFO / Vận hành):**
- Danh mục phản ánh chỉ có số nhóm hữu hạn, nên nhiều ca có thể giải quyết chỉ bằng rule và mô hình phân loại nhỏ. LLM chỉ đáng dùng cho phần mơ hồ, cần đo xem phần đó chiếm bao nhiêu.
- Chỉ số "chuyển nhầm 12%" cần lấy từ nhật ký chuyển phản ánh thật.

---

## 🗳️ Bài toán đề xuất để Deep-Dive

Tôi đề xuất **Card #1 (VinFast chẩn đoán lỗi)** hoặc **Card #3 (Vinhomes)** để nhóm thảo luận:

- **Card #1:** Giá trị rõ, đầu vào là ngôn ngữ tự nhiên nên hợp với LLM. Cần ranh giới an toàn nghiêm ngặt, đây là điểm hay để thể hiện tư duy Operational Boundary.
- **Card #3:** Dữ liệu dồi dào, rủi ro thấp hơn, dễ đo metric. Phù hợp nếu nhóm muốn bản mẫu chắc chắn.
- **Card #2:** Khả thi nhưng phụ thuộc vào việc có truy cập được hệ thống quản lý phòng hay không.
