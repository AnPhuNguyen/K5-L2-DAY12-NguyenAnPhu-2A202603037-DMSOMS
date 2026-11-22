# Danh sách lớp v0 — Nguyen An Phu · Gxx

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face_box` / `face` | hộp mặt; thuộc tính `eye_left`, `eye_right`, `glasses`, `gaze_zone` | Xác định tài xế đã xuất hiện và giới hạn vùng phân tích mắt, kính, hướng nhìn. Đây là đầu vào để đánh giá buồn ngủ, mất tập trung và đánh giá hành vi lái. | Có, nếu mặt đủ rõ và không bị che. Ghi `unknown` cho phần không phân biệt được. |
| 2 | `eye_left` / `eye_right` | `open` · `closed` · `unknown` | Cho biết từng mắt mở hay khép để ước lượng tình trạng nhắm mắt/thiểu tỉnh táo; cần gán riêng theo mắt của người, không theo bên trái ảnh. | Có thể nhìn được trên ảnh NIR thật, nhưng có thể bị che bởi chói mắt, kính râm hoặc mặt nghiêng; khi đó dùng `unknown`, không suy mắt còn lại. |
| 3 | `gaze_zone` | `road` · `mirror_l` · `mirror_r` · `mirror_c` · `cluster` · `center` · `lap` · `other` | Ước lượng tài xế đang nhìn đường, gương, bảng điều khiển, màn hình hay xuống đùi để phát hiện mất chú ý hoặc hành vi lái nguy hiểm. | Có thể suy ra từ hướng mắt và đầu nếu ảnh NIR đủ rõ; nếu không chắc chọn `other` hoặc ghi lý do. |
| 4 | `glasses` | `none` · `clear` · `sun` · `unknown` | Nhận biết tài xế đeo kính hay kính râm để xử lý chướng ngại quang học, tránh nhầm mắt mở/đóng hoặc ánh phản xạ là tình trạng mắt. | Có thể đoán nhầm trên IR do kính râm còn sáng; chỉ gán `clear`/`sun` khi thấy rõ, ngược lại dùng `unknown`. |
| 5 | `face13` | 13 điểm khuôn mặt; mỗi điểm có cờ `occluded` hoặc `outside` | Định vị mắt, mũi, khóe miệng và mép môi để theo dõi tư thế đầu, EAR/MAR hoặc phát hiện che khuất; chỉ dùng khi mặt rộng ít nhất 60 px. | Thường có trên ảnh NIR rõ, nhưng điểm che hoặc không đoán được phải đánh `occluded`/`outside`, không đặt điểm cho đủ. |
| 6 | `hand` | `whose`: `driver`/`passenger`; `side`: trái/phải của người; `on_wheel`: `yes`/`no`/`unknown` | Theo dõi tay tài xế có đang bám vô lăng hay thực hiện hành vi khác, đồng thời phân biệt tay hành khách. Trên xe tự lái, `on_wheel = no` là bình thường. | Có thể nhìn thấy phần bàn tay trên IR nếu độ tương phản đủ; tay bị tối, che hoặc không phân biệt bàn tay nào thì dùng `unknown` cho thuộc tính liên quan. |
| 7 | `phone` | `held` · `mounted` · `lap` | Phát hiện tài xế đang cầm, đặt trên giá hoặc để trên đùi; hỗ trợ cảnh báo sử dụng điện thoại trong khi lái hoặc điều khiển xe. | Thường chỉ gán khi ảnh IR thấy rõ ít nhất một cạnh, góc hoặc màn hình. Không gán chỉ dựa vào vật tối gần tai hoặc tay. |
| 8 | `camera_blocked` | `yes` · `no` | Phát hiện ống kính bị che để hệ thống biết dữ liệu DMS không đáng tin, thay vì tiếp tục đưa ra kết quả sai. Thuộc tính nằm trên tag `frame_meta`. | Có thể nhìn thấy bụi, tay hoặc vật che camera trên cả IR; chỉ gán `yes` khi che rõ, còn lại gán `no` nếu chắc camera không che. |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` | thuộc tính `seat`: `driver` · `front_passenger` · `rear_left` · `rear_middle` · `rear_right`; `seatbelt`: `worn_correct` · `worn_incorrect` · `not_worn` · `unknown` | Nhận diện hành khách, xác định vị trí ghế theo xe và kiểm tra trạng thái thắt dây an toàn để hỗ trợ phân phối người/vật hoặc cảnh báo an toàn. Trong OMS trên SVIRO chỉ dùng `not_worn`/`unknown`. | IR-sim có thể thấy người và vị trí ghế bằng hình dạng/ngữ cảnh, nhưng không nên đoán dây an toàn nếu hình ảnh không rõ; dùng `unknown` khi không thấy. |
| 2 | `seat` | `rear_left` · `rear_middle` · `rear_right` cho hàng ghế sau trong bài | Chuẩn hóa vị trí ghế theo hướng nhìn của người lái, không theo vị trí bên trái/phải trong ảnh. Giúp thống nhất dữ liệu cabin và so khớp ghế có người/vật. | Có thể nhận ra ghế qua bố cục nhưng nhiều ghế trống không có box; chỉ gán ghế có người/vật và thuộc tính `seat` trên box đó. |
| 3 | `seatbelt` | `worn_correct` · `worn_incorrect` · `not_worn` · `unknown` | Kiểm tra hành khách có thắt dây đúng cách hay không để hệ thống an toàn hoặc cảnh báo ghế. | Không nhìn thấy rõ thì `unknown`; trong SVIRO IR-sim theo guideline không ai thắt dây nên chỉ `not_worn` hoặc `unknown`, không gán `worn_correct`. |
| 4 | `child_seat` | `seat`; `occupied`: `no` · `unknown` | Nhận diện ghế trẻ em đang trống hoặc trạng thái không xác định; không dùng `occupied = yes`. Nếu có đồ vật trên ghế, gán hai box riêng. | Có thể nhận ra bằng hình dạng ghế và vị trí trong IR-sim; nếu phần bên trong bị che hoặc không phân biệt được dùng `occupied = unknown`. |
| 5 | `infant_seat` | `seat`; `facing`: `rear` · `front` | Nhận diện nôi/ghế em bé mang theo và hướng quay để hỗ trợ thống kê chỗ ngồi và kiểm tra bố trí cabin. | Có thể nhìn rõ hướng ghế khi hình nôi có hình dạng phân biệt; trường IR-sim bị che hoặc mơ hồ hướng quay phải hỏi guideline vì thuộc tính này không có giá trị `unknown`. |
| 6 | `object` | `seat`; `type`: `bag` · `box` · `cup` · `other` | Nhận diện đồ mang theo trong cabin để xem ghế nào bị chiếm dụng hoặc vật nào có thể gây che khuất/ảnh hưởng an toàn. | IR-sim có thể nhận vật qua hình dạng, kích thước và ngữ cảnh; nếu không chắc loại, gán `other` thay vì đoán `bag`/`box`/`cup`. |
| 7 | `frame_meta` / `modality` | `rgb` · `ir` | Ghi nguồn cảm biến để tách ảnh màu RGB, IR mô phỏng và IR thật; tránh áp sai kinh nghiệm IR thật lên bộ IR-sim hoặc ngược lại. | Chính trường này mô tả ảnh IR-sim/IR thật, không phải nội dung trực quan cần nhận diện. |
| 8 | `frame_meta` / `light` · `camera_blocked` | `light`: `day` · `night` · `unknown`; `camera_blocked`: `yes` · `no` | Ghi điều kiện ánh sáng và trạng thái che camera để giải thích độ tin cậy nhãn, lọc dữ liệu suy luận kém và chống cảnh báo sai. Với ảnh dựng SVIRO thường dùng `light = unknown`. | Trên IR-sim không suy ánh sáng thực; dùng `unknown`. Với IR thật, nhìn được vật che ống kính mới gán `camera_blocked = yes`, ngược lại `no` nếu chắc. |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Khi không nhìn thấy toàn bộ điểm khuôn mặt do kính râm, tóc hoặc vật che, điểm đó nên là `occluded` hay `outside`?
   Khung ví dụ: `face13` trên ảnh D04 khi sống mũi hoặc tròng kính che một khóe mắt.
   Bạn tạm gán thế nào và vì sao: Gán `occluded` nếu có thể suy chắc vị trí điểm ở mép mắt hoặc khuôn mặt; gán `outside` nếu không thể đoán vị trí đáng tin cậy. Không đặt điểm "cho đủ".
2. Câu hỏi: Khi một vật nhỏ nằm gần tai tài xế trên ảnh NIR, vật đó phải được gán `phone` hay không gán?
   Khung ví dụ: `phone` trên ảnh D05 với tay che phần lớn vật và không thấy rõ cạnh hoặc màn hình.
   Bạn tạm gán thế nào và vì sao: Không gán `phone` vì guideline yêu cầu thấy ít nhất một phần hình dạng điện thoại; nếu không chắc thì ghi ghi chú và để vật đó không thuộc lớp DMS.
