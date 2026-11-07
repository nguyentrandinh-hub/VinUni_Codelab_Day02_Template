# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)
### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 |**Vin Mec** | Lặp lại|Tự động hóa nhập liệu hồ sơ bệnh nhân: Nhân viên phải nhập và đối chiếu thông tin từ nhiều giấy tờ, kết quả xét nghiệm, đơn thuốc. AI OCR kết hợp LLM có thể trích xuất, chuẩn hóa và kiểm tra dữ liệu. |
| 2 |**Vin Mec** |Tốn thời gian |Tự động kiểm tra hồ sơ thanh toán bảo hiểm: Nhân viên phải rà soát chứng từ, hóa đơn, thông tin điều trị và điều kiện bảo hiểm. AI có thể phát hiện thiếu giấy tờ, sai lệch thông tin và phân loại hồ sơ cần xử lý. |
| 3 |**Vin Mec** |AI-upgrade |Trợ lý AI tư vấn và điều phối lịch khám: Nhân viên tiếp nhận nhiều câu hỏi về chuyên khoa, bác sĩ, lịch khám, quy trình và dịch vụ. AI chatbot có thể giải đáp câu hỏi thường gặp, hỗ trợ tìm chuyên khoa và đặt lịch khám. |
| 4 |**Xanh SM** |Lặp lại |Tự động phân loại và xử lý phản ánh của tài xế: Bộ phận vận hành phải tiếp nhận, phân loại và chuyển tiếp các phản ánh về chuyến xe, doanh thu, ứng dụng và sự cố. AI có thể phân loại nội dung, xác định mức độ ưu tiên và chuyển đúng bộ phận. |
| 5 |**Xanh SM** |Pain từ người khác |Tự động xác thực báo cáo lỗi bản đồ và điều kiện giao thông: Tài xế phản ánh đường cấm, đường mới, ngập nước, tắc đường hoặc chỉ dẫn sai. AI có thể phân tích nội dung, hình ảnh và vị trí báo cáo để hỗ trợ xác thực, ưu tiên cập nhật bản đồ. |

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)
```
## Thẻ bài toán tiêu biểu: 

### Card #01 – VINMEC - Tự động trích xuất và nhập liệu hồ sơ bệnh nhân từ giấy tờ, phiếu khám và kết quả xét nghiệm bằng AI.

┌──────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #01                                   │
│                                                          │
│ Bài toán: Tự động trích xuất và nhập liệu hồ sơ bệnh nhân│
│ từ giấy tờ, phiếu khám và kết quả xét nghiệm bằng AI.    │
│                                                          │
│ Công ty thành viên: [x] Vinmec                            │
│                                                          │
│ Ai đang đau? Nhân viên tiếp đón, nhân viên nhập liệu,    │
│ điều dưỡng và bộ phận quản lý hồ sơ bệnh án.             │
│                                                          │
│ Workflow thủ công hiện tại (5 bước):                     │
│   1. Tiếp nhận giấy tờ, ảnh chụp hoặc PDF từ bệnh nhân   │
│   → 2. Nhân viên đọc và tìm thông tin trên tài liệu      │
│   → 3. Nhập thủ công dữ liệu vào hệ thống HIS/EMR        │
│   → 4. Đối chiếu thông tin vừa nhập với tài liệu gốc     │
│   → 5. Sửa lỗi, hoàn thiện và lưu hồ sơ                  │
│                                                          │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 5-10 phút/tài liệu)       │
│                                                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3           │
│ (OCR đọc tài liệu → LLM trích xuất thông tin →           │
│ Chuẩn hóa JSON → Kiểm tra dữ liệu → Nhân viên xác nhận)  │
│                                                          │
│ Đo thành công bằng gì (Metric có số)?                    │
│ Giảm thời gian nhập liệu từ 5-10 phút → dưới 2 phút.    │
│ Độ chính xác trích xuất thông tin đạt ≥ 95%.             │
│                                                          │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent│
│                                                          │
│ OCR + LLM + Validation + Human-in-the-loop               │
└──────────────────────────────────────────────────────────┘

### CARD #02 – VINMEC - Tự động kiểm tra tính đầy đủ và phát hiện sai lệch thông tin trong hồ sơ thanh toán bảo hiểm. 

┌──────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #02                                   │
│                                                          │
│ Bài toán: Tự động kiểm tra tính đầy đủ và phát hiện      │
│ sai lệch thông tin trong hồ sơ thanh toán bảo hiểm.      │
│                                                          │
│ Công ty thành viên: [x] Vinmec                            │
│                                                          │
│ Ai đang đau? Nhân viên thanh toán viện phí, nhân viên    │
│ bảo hiểm, kế toán và bộ phận kiểm soát hồ sơ.            │
│                                                          │
│ Workflow thủ công hiện tại (5 bước):                     │
│   1. Tiếp nhận bộ hồ sơ thanh toán bảo hiểm              │
│   → 2. Kiểm tra các giấy tờ, chứng từ cần thiết          │
│   → 3. Đối chiếu mã bệnh nhân, ngày khám, dịch vụ,       │
│        hóa đơn và thông tin bảo hiểm                     │
│   → 4. Phát hiện sai lệch, yêu cầu bổ sung giấy tờ       │
│   → 5. Hoàn thiện và chuyển hồ sơ sang bước xử lý tiếp   │
│                                                          │
│ Bước nào tốn nhất? Bước 2-4 (⏱ 10-20 phút/hồ sơ)        │
│                                                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4           │
│ (OCR đọc chứng từ → LLM trích xuất dữ liệu →             │
│ Rule Engine đối chiếu checklist → AI báo lỗi/thiếu       │
│ giấy tờ → Nhân viên xác nhận)                            │
│                                                          │
│ Đo thành công bằng gì (Metric có số)?                    │
│ Giảm thời gian kiểm tra từ 10-20 phút → dưới 5 phút.    │
│ Độ chính xác phát hiện thiếu/sai chứng từ đạt ≥ 95%.    │
│                                                          │
│ Quick Architecture: [ ] No AI [x] Rule [x] LLM [ ] Agent│
│                                                          │
│ OCR + LLM + Rule Engine + Human-in-the-loop              │
└──────────────────────────────────────────────────────────┘

### CARD #03 – XANH SM - Tự động phân loại và điều phối phản ánh của tài xế Xanh SM đến đúng bộ phận phụ trách bằng AI.

┌──────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #03                                   │
│                                                          │
│ Bài toán: Tự động phân loại và điều phối phản ánh của    │
│ tài xế Xanh SM đến đúng bộ phận phụ trách bằng AI.       │
│                                                          │
│ Công ty thành viên: [x] Xanh SM (GSM)                    │
│                                                          │
│ Ai đang đau? Tài xế (chờ phản hồi), nhân viên hỗ trợ    │
│ tài xế và bộ phận vận hành (quá tải phản ánh).            │
│                                                          │
│ Workflow thủ công hiện tại (5 bước):                     │
│   1. Tài xế gửi phản ánh về doanh thu, chuyến xe,        │
│      tài khoản, ứng dụng hoặc phương tiện               │
│   → 2. Nhân viên đọc và tìm hiểu nội dung phản ánh       │
│   → 3. Phân loại vấn đề và xác định mức độ ưu tiên      │
│   → 4. Tạo ticket, chuyển đến bộ phận phụ trách         │
│   → 5. Theo dõi tiến độ và phản hồi lại tài xế           │
│                                                          │
│ Bước nào tốn nhất? Bước 2-4 (⏱ 3-5 phút/phản ánh)       │
│                                                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4           │
│ (LLM đọc nội dung → Phân loại vấn đề → Xác định         │
│ mức độ ưu tiên → Agent tạo ticket → Chuyển đúng bộ phận)│
│                                                          │
│ Đo thành công bằng gì (Metric có số)?                    │
│ Giảm thời gian phân loại từ 3-5 phút → dưới 30 giây.    │
│ Độ chính xác phân loại đạt ≥ 90%.                        │
│ Tỷ lệ chuyển đúng bộ phận đạt ≥ 95%.                    │
│                                                          │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [x] Agent│
│                                                          │
│ LLM Classification + Rule Routing + AI Agent + Ticket API│
└──────────────────────────────────────────────────────────┘
```