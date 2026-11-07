# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của các công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Lặp lại | Dự báo nhu cầu đặt xe theo khu vực và khung giờ để điều phối tài xế hợp lý, tránh tình trạng nơi thiếu xe, nơi tài xế phải chờ lâu. |
| 2 | **VinFast** | Lặp lại | Tự động kiểm tra lỗi ngoại quan trên dây chuyền sản xuất như trầy xước, lỗi sơn, móp méo và sai lệch linh kiện bằng Computer Vision. |
| 3 | **Vinhomes** | Lặp lại | Tự động phân loại phản ánh của cư dân về điện nước, thang máy, vệ sinh, an ninh và chuyển yêu cầu đến đúng bộ phận xử lý. |
| 4 | **Vinmec** | Tốn thời gian | Tối ưu lịch khám bằng cách dự đoán thời lượng khám và khả năng bệnh nhân không đến, hạn chế tình trạng bệnh nhân phải chờ lâu. |
| 5 | **VinFast** | AI-upgrade | Phân tích dữ liệu pin và hệ thống BMS để phát hiện dấu hiệu bất thường, dự đoán nguy cơ hỏng hóc và chủ động lên lịch bảo trì xe điện. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN: **#3 (Vinhomes CSKH), #1 (Xanh SM Nhu cầu đặt xe), #2 (VinFast Kiểm tra lỗi dây truyền sản xuất).**

## Thẻ bài toán tiêu biểu: Card #3 — Vinhomes Tự động phân loại và điều phối phản ánh cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
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

