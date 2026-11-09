# Báo cáo cá nhân — Ngày 2 Object Detection
## 1. Thông tin học viên

- Họ tên: Nguyễn Viết Minh Hoàng
- MSSV: 2A202603281
- Email: mihhoang307@gmail.com
- Lớp: L2B
- Link repo bài làm: https://github.com/hoangnvm307/K5-L2B-DAY02-NguyenVietMinhHoang-2A202603281-ObjectDetection
- Link Colab đã chạy:https://colab.research.google.com/drive/1KvS3ViTvoEgtBnh0Pn4Idm6dlzLeZrll#scrollTo=3e0508d8

## 2. Schema và mini guideline

Schema: `car=0`, `truck=1`, `bus=2`, `van=3`; mỗi xe thuộc phạm vi bài có một box ôm sát phần nhìn thấy. [Mini guideline](../../GUIDELINE_MINI.md) ghi ba ca đã xem: `drive_014` chọn `truck` vì nhận diện đầu kéo, `drive_044` chọn `car` cho xe sedan bị mờ, `drive_049` chọn `bus` cho xe dài ở xa.

## 3. Annotation và self-QC

- Số file nhãn train: **50/50** (`dataset/labels/train/`).
- Kết quả checker: `python -X utf8 tools/check_yolo_labels.py` báo **ĐẠT cấu trúc 50/50**.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_01-drive_011.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: ôm sát box hơn ở các vật thể nhỏ.

## 4. Gold QC và rework

| Chỉ số | Bản xuất đầu tiên | Bản CVAT lần 3 đang dùng |
|---|---:|---:|
| TP | 449 | 387 |
| FP | 586 | 327 |
| FN | 204 | 266 |
| Precision | 0,4338 | 0,5420 |
| Recall | 0,6876 | 0,5926 |
| F1 | 0,5320 | 0,5662 |
| mean_iou_of_matches | 0,8161 | 0,8272 |
| passed | Không theo ngưỡng cũ 0,80 | Có theo ngưỡng mới 0,50 |

[Kết quả annotation hiện tại](../../outputs/annotation_evaluation.json) được chạy với `--min-f1 0.50 --min-mean-iou 0.70`, theo ngưỡng mới do giảng viên thông báo . Sau khi giảm số box, FP giảm nhưng FN tăng; cần đối chiếu ảnh cụ thể trước khi kết luận nguyên nhân. 

## 5. Kết quả YOLO

- Runtime: Google Colab, **CPU** (`args.yaml` ghi `device: cpu`). Phiên bản Ultralytics: 8.4.208,Torch: 2.14.1+cpu.
- Tham số train ghi trong `args.yaml`: `yolo26n.pt`, `epochs=50`, `imgsz=640`, `batch=-1`. Các giá trị này khớp cell train của notebook; không thấy thay đổi so với cấu hình mặc định của bài.
- Best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0,4527 |
| Recall | 0,2437 |
| mAP50 | 0,2706 |
| mAP50–95 | 0,1948 |

Hai folder kết quả Colab: [10 ảnh prediction](assets/eval_predictions/visuals/) và [log train/eval](assets/yolo26_runs/). Ví dụ [drive_058](assets/eval_predictions/visuals/drive_058.jpg): xe buýt nhỏ gần mép phải ảnh được dự đoán thành `car`; đối chiếu với [confusion matrix](assets/yolo26_runs/vehicle_eval/confusion_matrix_normalized.png) khi nhận xét lỗi lớp.

Trả lời bốn câu hỏi cuối notebook:

1. Theo confusion matrix trên tập eval, `bus` có recall thấp nhất: không có ô đúng trên đường chéo cho hai bus trong eval. [drive_058](assets/eval_predictions/visuals/drive_058.jpg) minh họa xe buýt nhỏ gần mép phải bị dự đoán thành `car`. 
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm cạnh box có ôm sát xe không: box lệch hoặc quá rộng vẫn có thể khớp tại IoU 0,50 nhưng hụt ở các ngưỡng IoU cao hơn.
3. Nhầm `bus` với `car` có thể liên quan quy tắc phân biệt lớp và số mẫu ít. Confusion matrix cho thấy một phần bus thật được dự đoán là `car`, phần còn lại không khớp dự đoán. Tập eval chỉ có hai bus nên chưa đủ để kết luận guideline là nguyên nhân duy nhất.
4. Không đưa 10 ảnh eval vào train vì sẽ làm lẫn dữ liệu huấn luyện với dữ liệu đánh giá. Notebook dùng chúng làm validation trong quá trình train và chọn `best.pt`, nên đây không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Xuất nhãn CVAT | Bản xuất đầu chỉ có `train.txt` và `data.yaml` | Xuất lại ở định dạng Ultralytics YOLO Detection | Nhận đủ 50 file nhãn `.txt` |
| Chạy checker trên Windows | Console báo lỗi mã hóa khi in tiếng Việt | Chạy `python -X utf8 tools/check_yolo_labels.py` | Checker báo ĐẠT 50/50 |

## 7. Tự đánh giá theo rubric

Đã có 50 file nhãn đúng cấu trúc, kết quả gold QC theo ngưỡng mới và kết quả YOLO trên 10 ảnh eval. 

## 8. Công cụ AI và tài liệu tham khảo

Đã dùng Codex để đọc hướng dẫn, kiểm tra cấu trúc nhãn, chạy evaluator, soạn nháp mini guideline và tổng hợp kết quả Colab. 
