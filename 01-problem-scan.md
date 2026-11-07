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

Chọn top 3 từ bảng SCAN: **#1 (VinFast kiểm tra và đánh giá sự cố xe), #2 (Vinpearl Group Booking), #3 (Vinhomes phản ánh cư dân).**

Ba bài này khác nhau về kiểu kiến trúc (LLM, LLM kết hợp rule, rule kết hợp LLM) và mức rủi ro, nên dễ so sánh khi nhóm thảo luận.

## Card #1 — VinFast: Kiểm tra và đánh giá sự cố xe

```text
QUICK PROBLEM CARD #1

Bài toán (1 câu): Khi khách mang xe có sự cố đến xưởng, việc kiểm tra
và đánh giá nguyên nhân ban đầu phụ thuộc nhiều vào kinh nghiệm kỹ
thuật viên và mất khoảng 20 phút/xe; cần AI hỗ trợ tổng hợp triệu
chứng, lịch sử xe để gợi ý nguyên nhân khả dĩ.

Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes
                    [ ] Vinmec   [ ] Khác

Ai đang đau (Actor)?
  - Nhân viên tiếp nhận (hỏi thông tin, ghi triệu chứng bằng tay)
  - Kỹ thuật viên (tự tổng hợp triệu chứng, lịch sử, kết quả kiểm tra)
  - Khách hàng (chờ lâu mới biết nguyên nhân và báo giá)

Workflow thủ công hiện tại (6 bước, ~20 phút/xe):
  1. Nhân viên tiếp nhận hỏi thông tin khách, tra VIN và lịch sử bảo dưỡng
  -> 2. Nhân viên tiếp nhận hỏi khách về vấn đề của xe
  -> (handoff: chuyển thông tin xe/triệu chứng cho kỹ thuật viên)
  -> 3. Kỹ thuật viên quan sát, kiểm tra trực tiếp và chạy thử
  -> 4. Kỹ thuật viên đánh giá nguyên nhân từ kết quả kiểm tra và kinh nghiệm
  -> 5. Kỹ thuật viên thông báo lỗi, phương án sửa và báo giá
  -> (handoff: trao đổi phương án và báo giá với khách)
  -> 6. Khách hàng quyết định có đồng ý sửa hay không

Bước nào tốn thời gian/lỗi nhất? Bước 4: tổng hợp triệu chứng, lịch sử
liên quan và kết quả kiểm tra để hình thành đánh giá sơ bộ
(ước tính ~6 phút/20 phút, cần xác minh bằng đo thực tế)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (chuẩn hóa triệu chứng
khách mô tả thành dữ liệu có cấu trúc) và bước 4 (gợi ý nguyên nhân khả
dĩ từ triệu chứng + lịch sử bảo dưỡng của VIN, kỹ thuật viên quyết định).

Đo thành công bằng gì (Metric có số)? (ước tính, cần baseline thật)
  - Giảm thời gian từ lúc nhận xe đến lúc có đánh giá sơ bộ
    từ ~20 phút xuống dưới 12 phút/xe
  - Tỉ lệ gợi ý nguyên nhân của AI nằm trong top 3 kết luận cuối của
    kỹ thuật viên đạt từ 80% trở lên

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
  LLM chỉ gợi ý. Kỹ thuật viên luôn là người kết luận.
  Rule cứng chạy trước cho nhóm lỗi an toàn.
```

**Ranh giới an toàn:** Nếu triệu chứng có dấu hiệu liên quan phanh, lái, pin cao áp, túi khí hoặc cháy, hệ thống không để AI đưa ra gợi ý như bình thường mà báo ngay cho kỹ thuật viên/kỹ sư phụ trách và nhắc khách dừng xe an toàn. Đây là rule cứng chạy trước LLM. AI không báo giá và không quyết định có sửa hay không.

**Stress-test (CFO / Vận hành):**
- Rule theo từ khóa có đủ không? Với mô tả đời thường rất đa dạng thì khó, nhưng cần chạy thử trên phiếu cũ để so sánh với LLM trước khi chọn.
- Chỉ số "top 3 đạt 80%" cần dữ liệu phiếu kiểm tra cũ để làm baseline, hiện chưa có. Con số 20 phút/xe cũng cần đo thực tế.
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

## 🗳️ Bài toán được chọn để Deep-Dive

Nhóm đã chọn **Card #1 (VinFast kiểm tra và đánh giá sự cố xe)** để Deep-Dive. Lý do so với hai card còn lại:

- **Card #1:** Giá trị rõ, đầu vào là ngôn ngữ tự nhiên nên hợp với LLM. Cần ranh giới an toàn nghiêm ngặt, đây là điểm hay để thể hiện tư duy Operational Boundary.
- **Card #3:** Dữ liệu dồi dào, rủi ro thấp hơn, dễ đo metric. Phù hợp nếu nhóm muốn bản mẫu chắc chắn.
- **Card #2:** Khả thi nhưng phụ thuộc vào việc có truy cập được hệ thống quản lý phòng hay không.
