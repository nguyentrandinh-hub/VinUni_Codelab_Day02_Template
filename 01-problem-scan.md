# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Bài toán của tôi

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Lặp lại | Dự báo nhu cầu đặt xe theo khu vực và khung giờ để điều phối tài xế hợp lý, tránh tình trạng nơi thiếu xe, nơi tài xế phải chờ lâu. |
| 2 | **VinFast** | Lặp lại | Tự động kiểm tra lỗi ngoại quan trên dây chuyền sản xuất như trầy xước, lỗi sơn, móp méo và sai lệch linh kiện bằng Computer Vision. |
| 3 | **Vinhomes** | Lặp lại | Tự động phân loại phản ánh của cư dân về điện nước, thang máy, vệ sinh, an ninh và chuyển yêu cầu đến đúng bộ phận xử lý. |
| 4 | **Vinmec** | Tốn thời gian | Tối ưu lịch khám bằng cách dự đoán thời lượng khám và khả năng bệnh nhân không đến, hạn chế tình trạng bệnh nhân phải chờ lâu. |
| 5 | **VinFast** | AI-upgrade | Phân tích dữ liệu pin và hệ thống BMS để phát hiện dấu hiệu bất thường, dự đoán nguy cơ hỏng hóc và chủ động lên lịch bảo trì xe điện. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

## Thẻ bài toán tiêu biểu: 

Card #3 — Vinhomes Tự động phân loại và điều phối phản ánh cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Tự động phân loại phản ánh của cư dân và chuyển   |
| yêu cầu đến đúng bộ phận phụ trách nhằm giảm thời gian xử lý|
| thủ công.                                                   │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau?                                                │
│ - Cư dân: Phải chờ phản hồi khi gửi yêu cầu hoặc báo cáo    |
| sự cố.                                                      |
| - Nhân viên CSKH: Mất thời gian đọc, phân loại và chuyển    |
| tiếp phản ánh.                                              |
| - Nhân viên kỹ thuật: Có thể nhận yêu cầu sai bộ phận hoặc  | 
| thiếu thông tin.                                            |
|                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│  1. Cư dân gửi phản ánh qua ứng dụng, hotline hoặc email.   |
|  2. Nhân viên CSKH đọc và kiểm tra nội dung phản ánh.       |
|  3. Phân loại vấn đề: điện nước, thang máy, vệ sinh,        |
|     an ninh...                                              |
|  4. Xác định mức độ ưu tiên và chuyển yêu cầu đến bộ phận   |
|     phụ trách.                                              |
|  5. Nhân viên phụ trách xử lý và phản hồi kết quả cho cư    |
|     dân.                                                    │
│                                                             │
│ Bước nào tốn nhất? Bước 2-4 (⏱ 5-10 phút/lượt)             |
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4              │
│ Sử dụng NLP/LLM để đọc nội dung, tự động phân loại phản ánh,|
| nhận diện mức độ khẩn cấp và đề xuất bộ phận xử lý.         |
| Nhân viên xác nhận trước khi chuyển ticket.                 │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian phân loại và chuyển yêu cầu từ 5–10 phút     |
| xuống dưới 1 phút.                                          |
| Độ chính xác phân loại ticket mục tiêu ≥ 90%.               |
| Giảm tỷ lệ chuyển nhầm bộ phận xuống dưới 5%.               │
│                                                             │
│ Quick Architecture: [x] LLM Feature [x] Rule                │
└─────────────────────────────────────────────────────────────┘
```

Card #1 — Xanh SM Nhu cầu đặt xe theo khu vực và khung giờ để điều phối tài xế hợp lý

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Dự báo nhu cầu đặt xe theo khu vực và khung giờ để|
| hỗ trợ điều phối tài xế, hạn chế tình trạng thiếu xe và tài |
| xế phải chờ lâu                                             │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau?                                                │
│ - Tài xế: Phải chờ lâu tại khu vực ít khách.                |
| - Bộ phận điều phối: Khó dự đoán khu vực có nhu cầu tăng    |
|   cao.                                                      |
| - Khách hàng: Phải chờ lâu khi thiếu tài xế.                | 
|                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│  1. Thu thập dữ liệu chuyến xe và nhu cầu đặt xe.           |
|  2. Theo dõi số lượng tài xế đang hoạt động theo khu vực.   |
|  3. Phân tích thủ công khu vực có nhu cầu cao/thấp.         |
|  4. Ước lượng nhu cầu đặt xe trong các khung giờ tiếp theo. |
|  5. Đề xuất tài xế di chuyển đến khu vực có nhu cầu cao.    |
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 10-15 phút/lượt)            |
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ Tự động phân tích dữ liệu lịch sử, dự báo nhu cầu đặt xe    |
| theo khu vực và khung giờ, đề xuất vị trí điều phối tài xế. |
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
| Forecast Accuracy mục tiêu ≥ 85%.                           |
| Giảm thời gian phân tích và dự báo từ 15 phút xuống dưới    |
| 2 phút.                                                     |
| Giảm thời gian tài xế chờ không có khách tối thiểu 10%.     │
│                                                             │
│ Quick Architecture: [x] Machine Learning                    │
└─────────────────────────────────────────────────────────────┘
```

Card #2 — VinFast Tự động kiểm tra lỗi ngoại quan trên dây chuyền sản xuất

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Tự động phát hiện lỗi ngoại quan trên xe và linh  |
| kiện trong dây chuyền sản xuất nhằm nâng cao độ chính xác và|
| giảm thời gian kiểm tra chất lượng.                         │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau?                                                │
│ - Nhân viên QC: Phải kiểm tra ngoại quan nhiều sản phẩm     |
|   liên tục.                                                 |
| - Nhân viên sản xuất: Phải kiểm tra và xử lý lại sản phẩm   |
|   khi phát hiện lỗi.                                        |
| - Bộ phận quản lý chất lượng: Khó kiểm soát đồng nhất chất  |
|   lượng kiểm định.                                          |
|                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│  1. Xe hoặc linh kiện đi qua công đoạn kiểm tra chất lượng. |
|  2. Nhân viên QC quan sát và kiểm tra ngoại quan sản phẩm.  |
|  3. Xác định các lỗi như trầy xước, móp méo, lỗi sơn hoặc   |
|     sai lệch linh kiện.                                     |
|  4. Ghi nhận lỗi và phân loại sản phẩm đạt/không đạt.       |
|  5. Chuyển sản phẩm lỗi đến bộ phận phụ trách để kiểm tra   |
|     và sửa chữa.                                            |
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 3-5 phút/lượt)              |
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ Sử dụng camera và Computer Vision để tự động nhận diện,     |
| khoanh vùng và phân loại lỗi ngoại quan trên sản phẩm.      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Defect Detection Accuracy mục tiêu ≥ 90%.                   |
| Giảm thời gian kiểm tra từ 5 phút xuống dưới                |
| 1 phút/sản phẩm                                             |
| - False Negative Rate mục tiêu dưới 5%.                     │
│                                                             │
│ Quick Architecture: [x] Computer Vision                     │
└─────────────────────────────────────────────────────────────┘
```