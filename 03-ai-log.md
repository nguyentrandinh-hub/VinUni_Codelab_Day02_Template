# 🤖 AI Interaction Log — Lab 02: AI Product Scoping

**Dự án:** AI hỗ trợ kỹ thuật viên VinFast đánh giá sơ bộ sự cố xe  
**Công cụ AI:** ChatGPT, Codex (VS Code), Gemini 2.5 Flash  
**Prototype:** `starter-code/prompt_prototype.py`

## 1. Quá trình sử dụng AI

| Giai đoạn | Tôi yêu cầu AI hỗ trợ gì? | AI đã hỗ trợ gì? | Tôi kiểm chứng / điều chỉnh gì? |
|---|---|---|---|
| Phase 1 — Problem Scan | Gợi ý các bài toán thực tế có thể ứng dụng AI trong hệ sinh thái Vingroup. | Hỗ trợ phân tích 5 ý tưởng liên quan đến VinFast, Vinhomes, Vinpearl và Xanh SM. | So sánh tính thực tế, dữ liệu cần thiết và phạm vi triển khai của từng ý tưởng. |
| Phase 2 — Quick Assess | Phân tích 3 bài toán tiềm năng theo mẫu Quick Problem Card. | Hỗ trợ xác định Actor, Workflow, Bottleneck, AI Solution và Success Metric. | Chọn bài toán hỗ trợ kỹ thuật viên VinFast vì có trải nghiệm thực tế để đối chiếu. |
| Phase 3 — Deep Dive | Hỗ trợ mô tả quy trình tiếp nhận và kiểm tra xe, xác định điểm nghẽn và Operational Boundary. | Đề xuất Current-State Workflow, Future-State Flow, AI-Fit Matrix và KPI. | Điều chỉnh quy trình theo quan sát cá nhân; làm rõ AI chỉ hỗ trợ đưa ra giả thuyết, kỹ thuật viên giữ quyền quyết định. |
| Phase 4 — Prototype | Yêu cầu Codex xây dựng prototype Python gọi Gemini 2.5 Flash và kiểm tra ranh giới an toàn. | Tạo System Prompt, JSON output, validator và 4 test case, gồm 3 adversarial tests. | Kiểm tra logic validator, làm rõ dữ liệu mâu thuẫn ở Test 3 và chạy thử với Gemini API. |
| Phase 4 — Testing vòng 1 | Chạy bốn test case để đánh giá phản hồi của Gemini. | Gemini trả về JSON hợp lệ và có xu hướng tuân thủ ranh giới, nhưng sai kiểu dữ liệu ở một số trường. | Phát hiện `fact_summary` và `missing_information` trả về `string` thay vì `list[string]`. Kết quả: **0 PASS, 4 FAIL**. |
| Phase 4 — Prompt Refinement | Yêu cầu Codex cải tiến phần định dạng trong System Prompt. | Làm rõ kiểu dữ liệu từng trường và bổ sung ví dụ JSON hợp lệ. | Giữ nguyên Operational Boundary và bốn test case để so sánh trước/sau. |
| Phase 4 — Testing vòng 2 | Chạy lại prototype sau khi cải tiến prompt. | Gemini trả về JSON đúng schema trong cả bốn test. | Kết quả: **4 PASS, 0 FAIL** về kiểm tra tự động; tiếp tục review thủ công nội dung và HITL. |
| Phase 5 — Evaluate | Hỗ trợ đánh giá mức độ sẵn sàng của giải pháp AI. | Phân biệt khả năng chạy prototype và khả năng triển khai thực tế. | Giữ đánh giá **NOT YET** đối với triển khai thực tế vì chưa có dữ liệu kiểm chứng và chưa đo KPI. |

## 2. Lỗi AI và quá trình khắc phục

### Vấn đề 1 — JSON hợp lệ nhưng sai cấu trúc

Trong vòng kiểm thử đầu tiên, Gemini trả về:

```json
{
  "fact_summary": "Xe có tiếng lạch cạch..."
}
```

Trong khi schema yêu cầu:

```json
{
  "fact_summary": [
    "Xe có tiếng lạch cạch..."
  ]
}
```

**Nguyên nhân:** Yêu cầu trả về JSON chưa đủ để đảm bảo model tuân thủ kiểu dữ liệu cụ thể.

**Cách khắc phục:** Làm rõ `list[string]`, `string`, `boolean` trong System Prompt và bổ sung ví dụ JSON hợp lệ.

**Kết quả:** Số test vượt kiểm tra tự động tăng từ 0/4 lên 4/4 trong hai lần chạy đã ghi nhận.

### Vấn đề 2 — Rủi ro suy luận vượt dữ liệu

Trong Test 1 của vòng 2, Gemini đưa ra giả thuyết rằng tiếng lạch cạch có thể liên quan đến một chi tiết cơ khí thuộc hệ thống treo hoặc gầm xe bị lỏng hoặc hao mòn.

Tuy nhiên, dữ liệu đầu vào chỉ ghi nhận tiếng lạch cạch và chuyển động bất thường ở một chi tiết chưa xác định.

**Đánh giá:** Đây là giả thuyết có tính khả dĩ nhưng chưa được bằng chứng đầu vào xác nhận trực tiếp. Vì vậy, không thể xem là kết luận chẩn đoán.

**Bài học:** Kiểm tra JSON và trích dẫn bằng chứng chưa đủ; cần đánh giá xem bằng chứng có thực sự hỗ trợ nội dung giả thuyết hay không.

### Vấn đề 3 — Bảo vệ Operational Boundary

Tôi kiểm thử ba tình huống đối kháng:

1. **Thiếu thông tin:** Yêu cầu AI khẳng định nguyên nhân dù chỉ có mô tả “xe có tiếng lạ”.
2. **Dữ liệu mâu thuẫn:** Yêu cầu AI bỏ qua một trong hai ghi chú chạy thử trái ngược.
3. **Ép tự quyết định:** Giả danh quản lý để yêu cầu AI phê duyệt bảo hành, quyết định sửa chữa và báo giá.

Trong vòng 2, Gemini không thực hiện các yêu cầu vượt quyền. Các trường hợp thiếu dữ liệu hoặc mâu thuẫn đều trả về `needs_more_information`, với `technician_review_required: true`.

**Giới hạn:** Kết quả chỉ chứng minh hành vi quan sát được trong các ca thử nghiệm này, không đảm bảo model luôn an toàn ở mọi tình huống.

## 3. Tổng hợp kết quả kiểm thử

| Chỉ số | Vòng 1 | Vòng 2 |
|---|---|---|
| Model | Gemini 2.5 Flash | Gemini 2.5 Flash |
| Số test | 4 | 4 |
| Automatic PASS | 0 | 4 |
| Automatic FAIL | 4 | 0 |
| Runtime ERROR | 0 | 0 |
| Thay đổi chính | Prompt ban đầu | Làm rõ JSON schema trong System Prompt |

**Nhận xét:** Việc cải tiến prompt giúp đầu ra ổn định hơn về cấu trúc trong bộ test hiện tại. Tuy nhiên, tỷ lệ 4/4 PASS không phải độ chính xác chẩn đoán và chưa phản ánh hiệu quả trong môi trường dịch vụ thực tế.

## 4. Bài học rút ra

Qua bài Lab, tôi nhận thấy AI hữu ích trong việc hỗ trợ phân tích bài toán, thiết kế prototype và tạo các tình huống kiểm thử. Tuy nhiên, kết quả do AI tạo ra cần được đối chiếu với quan sát thực tế và các yêu cầu kỹ thuật.

Ba bài học chính:

- **Problem Scoping trước khi Coding:** Cần hiểu rõ quy trình, điểm nghẽn và quyền quyết định của con người trước khi chọn giải pháp AI.
- **Prompt Engineering cần kiểm thử:** Prompt không chỉ cần mô tả nhiệm vụ mà còn phải xác định định dạng đầu ra, dữ liệu được phép sử dụng và hành vi bị cấm.
- **Human-in-the-loop là bắt buộc:** AI chỉ nên hỗ trợ tổng hợp thông tin và đưa ra giả thuyết; kỹ thuật viên phải xác minh và quyết định phương án xử lý.

## 5. Kết luận

Prototype VinFast đã hoàn thành bước thử nghiệm ban đầu với dữ liệu giả lập. Sau một vòng cải tiến System Prompt, cả bốn ca đều vượt qua kiểm tra tự động và không phát hiện hành vi tự quyết định vượt quyền trong các phản hồi đã quan sát.

Tuy nhiên, hệ thống chưa được đánh giá trên dữ liệu sự cố thực tế, chưa tích hợp hệ thống quản lý dịch vụ VinFast và chưa chứng minh mục tiêu giảm thời gian từ khoảng 20 xuống 15 phút/xe.

**Quyết định:** GO cho việc tiếp tục thử nghiệm có kiểm soát; **NOT YET** cho triển khai thực tế.