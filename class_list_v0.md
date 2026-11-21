# Danh sách lớp v0 — PhamVanNgocVinh · G01

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_state` | open / closed / unknown | Phát hiện tài xế ngủ gật, nhắm mắt kéo dài (microsleep) để kịp thời phát chuông cảnh báo | Có, trừ khi bị đốm phản xạ đèn LED NIR trên tròng kính |
| 2 | `gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện xao nhãng hướng nhìn (nhìn màn hình, điện thoại trên đùi quá lâu) | Có, nhờ tương phản rõ nét giữa tròng đen và lòng trắng dưới NIR |
| 3 | `head_pose` | yaw, pitch, roll (hoặc vùng nhìn đầu) | Ước lượng hướng chú ý tổng thể của tài xế và bổ trợ cho hướng mắt khi mắt bị che khuất | Rất tốt (đường nét giải phẫu và cấu trúc khuôn mặt rõ ràng) |
| 4 | `face13_landmarks` | 13 điểm mốc mặt | Tính toán tỉ lệ mở mắt EAR và mở miệng MAR để nhận diện tần suất chớp mắt, ngáp và biểu cảm mệt mỏi | Có, với điều kiện khuôn mặt đủ lớn (chiều rộng >= 60 px) |
| 5 | `hand_on_wheel` | yes / no / unknown | Phát hiện buông cả 2 tay khỏi vô lăng (hands-off detection) trong hệ thống hỗ trợ lái tự động L2/L2+ | Có, nhưng vùng vô lăng xa đèn LED có thể bị tối do ánh sáng suy giảm |
| 6 | `phone_state` | held / mounted / lap | Phát hiện vi phạm sử dụng điện thoại khi đang điều khiển phương tiện | Có nếu thấy cạnh viền/màn hình phản chiếu; khó thấy nếu bị lòng bàn tay che |
| 7 | `glasses` | none / clear / sun / unknown | Giúp thuật toán nhận diện mắt điều chỉnh tham số khử phản xạ kính phù hợp | Có, nhưng lưu ý kính râm có thể bị nhìn xuyên thấu dưới tia NIR |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `seat_occupancy` | empty / person / child_seat / infant_seat / object | Xác định trạng thái ghế để điều khiển kích hoạt hoặc ngắt túi khí (airbag suppression) tương ứng | Rất tốt thông qua hình học 3D và biên dạng khối |
| 2 | `infant_seat_facing` | rear / front | Bắt buộc TẮT túi khí ghế phụ nếu nôi em bé quay về sau (rear-facing) để tránh nguy cơ tử vong khi túi khí nổ | Có, nhận diện rõ quai xách và chiều vát của nôi |
| 3 | `child_presence` | present / absent | Hệ thống phát hiện bỏ quên trẻ em trong xe (Child Presence Detection theo chuẩn Euro NCAP) sau khi tắt máy | Có, hoạt động ổn định cả ban đêm khi xe đỗ trong bóng tối |
| 4 | `seatbelt_status` | worn_correct / worn_incorrect / not_worn / unknown | Cảnh báo thắt dây an toàn cho từng hành khách ở hàng ghế sau | Kém hơn RGB do mất tương phản màu sắc giữa dây đai và áo tối màu |
| 5 | `object_type` | bag / box / cup / other | Cảnh báo quên đồ đạc, hành lý khi rời xe; phân biệt đồ vật với sinh vật sống để tránh báo động giả | Có, nhận diện theo hình học khối hộp/balo, không dựa vào màu sắc |
| 6 | `passenger_posture` | normal / leaning_forward / lying_down | Hệ thống túi khí thông minh (Smart Airbag) điều chỉnh lực nổ phù hợp với tư thế ngồi của hành khách | Có, nhận diện qua tỷ lệ khung hộp bao cơ thể hoặc khung xương người |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. **Câu hỏi:** Khi một đồ vật nhỏ (ví dụ balo, túi xách) được đặt lọt hẳn vào bên trong ghế trẻ em `child_seat`, guideline yêu cầu vẽ 2 box riêng biệt. Trong trường hợp đồ vật che lấp hoàn toàn lòng ghế trẻ em, thuộc tính `occupied` của `child_seat` nên gán là `no` (không có trẻ em) hay `unknown` (bị che khuất không khẳng định được có trẻ bên dưới hay không)?
   - **Khung ví dụ:** `S01_rgb.png` / `S03_rgb.png`
   - **Bạn tạm gán thế nào và vì sao:** Tạm gán `occupied = no` vì bản chất bộ dữ liệu SVIRO không có trẻ em thật, và đồ vật bên trong đã được mô hình hóa độc lập bằng box `object` cùng toạ độ ghế.
2. **Câu hỏi:** Trên ảnh cận hồng ngoại NIR thật của DMS, nhiều loại kính râm (sunglasses) cho tia bước sóng 940nm đi qua gần như hoàn toàn khiến tròng kính trông trong suốt như kính cận. Thuộc tính `glasses` nên gán là `sun` (theo loại kính thực tế dựa vào gọng kính) hay gán `clear` / `unknown` (theo hình ảnh camera ghi nhận được)?
   - **Khung ví dụ:** `D01.png` / `D04.png`
   - **Bạn tạm gán thế nào và vì sao:** Tạm gán `glasses = unknown` nếu không thấy rõ đặc trưng của kính râm, nhằm tránh gây nhiễu cho mô hình phân loại kính khi làm việc với camera hồng ngoại ban đêm.
