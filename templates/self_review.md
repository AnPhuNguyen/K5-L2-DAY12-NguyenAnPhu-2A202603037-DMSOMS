# Tự đánh giá — <Họ tên> · <Gxx>

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S08_ir.png | ghế trẻ em có trẻ em ngồi ở trên | extra | child_seat: occupied='yes' không hợp lệ (cho phép: unknown, no) | đã sửa |
| S05_ir.png | ghế trẻ em có trẻ em ngồi ở trên | extra | child_seat: occupied='yes' không hợp lệ (cho phép: unknown, no) | đã sửa |
| D02.png | người đàn ông ở ghế lái, một tay trên vô lăng | ok |  |  |
| D01.png | cô gái ở ghế lái, uống một đồ uống | ok  |  |  |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: extra

Vì sao mình mắc: child_seat trong ảnh SVIRO chỉ cho phép attribute 'occupied' giá trị unknown hay no, nhưng bản thân lại thấy vật có trẻ em ngồi trên, do đó đặt giá trị yes

Lần sau tránh bằng cách: đọc kĩ guideline trước khi làm

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: …
