# Tự đánh giá — PhamVanNgocVinh · G01

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S01_rgb.png | infant_seat | seat_flip | Lúc đầu nhầm thành seat=driver, trong khi camera OMS nhìn hàng ghế sau, nôi bên trái ảnh ứng với ghế phải xe (rear_right) | Có |
| S03_rgb.png | child_seat | wrong_class | Nhầm thuộc tính occupied='yes', theo schema guideline child_seat chỉ nhận 'no' hoặc 'unknown' | Có |
| D01.png | person & face13 | ok | Box person ôm sát, 13 điểm keypoint đúng trái/phải theo người (trục Y mắt trái lớn hơn mắt phải trên ảnh xoay 90°) | Không cần |
| D06.png | face & face13 | flag | Thuộc tính eye=unknown khi mắt nhìn xuống đùi nhưng ban đầu chưa bật cờ occluded cho các keypoint mắt | Có |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Nhầm lẫn thuộc tính `occupied='yes'` cho ghế trẻ em `child_seat` và nhầm vị trí ghế theo hướng nhìn ảnh thay vì theo hướng xe chạy.

Vì sao mình mắc: Thói quen trực quan nghĩ rằng có ghế trẻ em tức là "occupied" và nhìn bên trái màn hình tưởng là bên trái xe.

Lần sau tránh bằng cách: Đọc kỹ schema định nghĩa các giá trị hợp lệ của thuộc tính, và luôn đặt mình vào vị trí người ngồi trên xe nhìn về phía trước để xác định đúng ghế trái/phải.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó:
Ở ảnh D06, bạn mình đã đánh cờ `occluded` cho keypoint mắt khi gán thuộc tính `eye_left/eye_right=unknown` do tài xế cúi đầu nhìn xuống đùi (`gaze_zone=lap`), trong khi mình ban đầu quên bật cờ này. Mình học được tính đồng bộ nghiêm ngặt giữa thuộc tính phân loại cấp cao (high-level attribute) và cờ trạng thái của điểm mốc cấp thấp (low-level landmark flag).
