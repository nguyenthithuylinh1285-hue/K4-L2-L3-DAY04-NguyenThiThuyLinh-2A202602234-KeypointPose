# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Thị Thùy Linh  
Mã SV: 2A202602234  
Nhóm: *(Bài làm cá nhân)*  
Ngày: 16/09/2026  

---

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 364 / 67 / 45 |
| Thời gian trung bình mỗi ảnh | ~4.0 phút/ảnh |

**Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):**

1. `left_ear`: **29%** (8 khớp v=1)
2. `left_knee`: **21%** (6 khớp v=1)
3. `left_eye`: **21%** (6 khớp v=1) / `right_ear`: **21%** (6 khớp v=1)

**Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích:**  
Các khớp trên phản ánh đúng đặc tính tư thế hay bị che khuất trong tập ảnh, nhưng không hẳn là những khớp khó xác định vị trí giải phẫu nhất. Tai và mắt là các vị trí giải phẫu rất rõ ràng trên hộp sọ, nhưng chúng có `%v=1` cao vì thường xuyên bị che khuất bởi góc mặt nghiêng, tóc, kính bảo hộ hoặc mũ bảo hiểm xe máy/xe cào cào (như ở `train_04`, `train_10`). Trong khi đó, khớp thực sự khó định vị nhất là **hông** (`left_hip`, `right_hip`) vì hoàn toàn bị quần áo che lấp và phải ước lượng tâm xoay xương đùi dựa trên giải phẫu học cơ thể.

---

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | **0.9366 (93.66%)** | — |
| OKS@0.50 | **0.9655 (96.55%)** | — |
| OKS@0.75 | **0.9310 (93.10%)** | — |
| Lỗi `dao_trai_phai` | **0** | — |
| Lỗi `nham_nguoi` | **0** | — |
| Lỗi `xoa_khop_bi_che` | **3** | — |

**Tôi đã sửa gì giữa hai lần chạy:**  
*(Bài thực hành chạy chấm 1 lần duy nhất với Gold, không tiến hành chạy lại lần 2 sau rework, giữ nguyên toàn bộ nhãn gốc đã nộp).*

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**  
Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh (kết quả ghi nhận 0 lỗi `dao_trai_phai`).

---

## 3. Kiểm chéo

Bạn cùng nhóm: *(Không thực hiện - Bài thực hành cá nhân)*

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:
- *(Để trống do không thực hiện kiểm chéo)*

---

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| `pose_mAP50` | 0.8450 | 0.8450 | +0.0000 |
| `pose_mAP50-95` | 0.6853 | 0.6908 | **+0.0055** |
| `pose_precision` | 0.9734 | 0.9792 | **+0.0058** |
| `pose_recall` | 0.8462 | 0.8462 | +0.0000 |
| `box_mAP50-95` | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook:

1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, hãy giải thích: 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**  
   Chỉ số `pose_mAP50-95` tăng **+0.0055** (từ 0.6853 lên 0.6908) và `pose_precision` tăng **+0.0058**. Bộ 20 ảnh train đạt chất lượng gán nhãn rất tốt (OKS đạt 93.66%) với cờ `v=1` được áp dụng đúng chuẩn cho các khớp bị che khuất (như người lái mô-tô, người đội mũ bảo hiểm), giúp mô hình học cách tinh chỉnh toạ độ khớp chính xác hơn trong các tư thế bị che lấp. Ngược lại, `box_mAP50-95` giảm nhẹ (-0.0078) vì tập train 20 ảnh có nhiều ảnh chụp cận cảnh/bị cắt ngang người, khiến mô hình bị lệch nhẹ phân phối bounding box tổng thể so với phân phối COCO ban đầu.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm người dễ hơn hay tìm khớp dễ hơn? Vì sao?**  
   `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) khoảng **0.1133** (~11.3%). Model tìm người **dễ hơn nhiều** so với tìm khớp. Bounding box của người bao quát diện tích lớn với các đặc trưng tổng thể rõ ràng (đầu, thân, quần áo) nên ít nhạy cảm với biến dạng cục bộ; trong khi keypoint là các điểm pixel đơn lẻ, đòi hỏi độ chính xác giải phẫu cực cao và rất dễ bị lệch khi khớp bị che khuất, xoay nghiêng hoặc nếp gấp trang phục làm nhiễu.

3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43:**  
   Trên tập test (ví dụ ở ảnh `test_03.jpg`), khi người ngồi nghiêng và bị khuất một phần tay, model mắc lỗi **Trượt hẳn (Gross offset)** ở khớp cổ tay: điểm dự đoán bị lệch ra ngoài vị trí giải phẫu và bám vào nếp gấp trang phục thay vì cổ tay thực tế.

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**  
   Ảnh `train_06.jpg` có OKS giữa nhãn và model thấp nhất (**0.597**). **Nhãn của bạn đúng.** Căn cứ vào file đối chiếu Gold `eval_vs_gold.json`, nhãn của bạn ở `train_06.jpg` đạt điểm OKS so với nhãn chuẩn Gold lên tới **0.9037** (90.37%). Model bị OKS thấp ở ảnh này do hậu cảnh phức tạp và tư thế người khiến model dự đoán trượt các khớp bên phải cơ thể.

5. **Ảnh bạn gán tệ nhất có cũng là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**  
   Ảnh bạn có OKS thấp nhất với Gold là `train_04.jpg` (OKS người thứ 2 là 0.7255) và `train_10.jpg` (OKS = 0.7982). So với model, các ảnh này model cũng gặp khó khăn (OKS khoảng 0.83). Điều này cho thấy đây là những **bức ảnh có độ mơ hồ cao thực tế**: người đội mũ bảo hiểm full-face che kín toàn bộ đầu/tai, đồng thời thân dưới bị cắt mép ảnh hoặc che bởi xe máy, khiến cả người gán nhãn lẫn mô hình học máy đều gặp thách thức lớn trong việc định vị chính xác khớp.

---

## 5. Một rule evidence bạn đã dùng

- **Đối tượng:** Ảnh `train_10.jpg`, người thứ 1, khớp `right_hip` (hông phải).
- **Bằng chứng thị giác:** Người trong ảnh chụp cận nửa thân trên, toàn bộ phần xương chậu và chân nằm ngoài mép dưới bức ảnh (toạ độ ban đầu chớm ra ngoài `y = 1.0005`).
- **Lý do quyết định:** Dù biết vị trí giải phẫu của hông nằm ngay sát phía dưới mép áo, nhưng theo quy tắc mép ảnh: khớp nằm ngoài khung hình không còn bất kỳ pixel nào biểu diễn trong ảnh thì bắt buộc phải mang cờ `v=0` (Outside) và toạ độ đưa về `(0.0, 0.0)`, không được để `v=1` ước lượng ngoài ảnh.
