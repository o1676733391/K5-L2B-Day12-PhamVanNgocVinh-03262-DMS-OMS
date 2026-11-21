# Khai báo dùng AI — Phạm Văn Ngọc Vinh

Không dùng AI thì xóa bảng và ghi: **Không dùng.**

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Antigravity AI Assistant | Giải thích hệ thống nhãn và quy ước Guideline Cabin v1.0 | Nắm vững quy ước gọi ghế theo xe, trái/phải theo người và quy tắc gán thuộc tính không đoán | Kiểm lại bằng GUIDE.md và labels_cabin_v1.json |
| 2 | Antigravity AI Assistant | Hỗ trợ cấu hình Docker / CVAT và kết nối Traefik | Khởi động lại dịch vụ CVAT local thành công trên cổng 8080 | Kiểm tra qua trình duyệt localhost:8080 |
| 3 | Antigravity AI Assistant | Hỗ trợ chạy script tự kiểm cabin_check.py và phân tích các lỗi ERROR/WARN | Sửa các lỗi occupied của child_seat và thuộc tính light/seat cho ảnh SVIRO | Kiểm chứng lại bằng lệnh python scripts/cabin_check.py đạt 0 lỗi 0 cảnh báo |
