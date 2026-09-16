# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 15.39 khớp có v > 0 mỗi người
- Tổng: v=2 364 | v=1 67 | v=0 45

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 5 | 0 | 18% |
| 1 | left_eye | 20 | 6 | 2 | 21% |
| 2 | right_eye | 22 | 5 | 1 | 18% |
| 3 | left_ear | 11 | 8 | 9 | 29% |
| 4 | right_ear | 17 | 6 | 5 | 21% |
| 5 | left_shoulder | 27 | 1 | 0 | 4% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 24 | 3 | 1 | 11% |
| 8 | right_elbow | 25 | 3 | 0 | 11% |
| 9 | left_wrist | 22 | 5 | 1 | 18% |
| 10 | right_wrist | 24 | 3 | 1 | 11% |
| 11 | left_hip | 23 | 4 | 1 | 14% |
| 12 | right_hip | 24 | 3 | 1 | 11% |
| 13 | left_knee | 19 | 6 | 3 | 21% |
| 14 | right_knee | 21 | 4 | 3 | 14% |
| 15 | left_ankle | 18 | 1 | 9 | 4% |
| 16 | right_ankle | 17 | 3 | 8 | 11% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
