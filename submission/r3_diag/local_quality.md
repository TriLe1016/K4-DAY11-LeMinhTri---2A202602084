# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `3c780f206e1c3d11c64be26982b4093b69c1b105db0175b1034259b9b3bb2c48`; slice `B2-mid`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_060000.jpg, adasind_086220.jpg, adasind_102750.jpg. Frame thiếu trong export: không.
TP=15; FP=4; FN=5; số lần đối chiếu=22; mean IoU của TP=0.851.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.682 | 0.898 | 0.818 |
| precision | 0.789 | 0.882 | 0.727 |
| recall | 0.750 | 0.681 | 0.333 |
| jaccard | 0.625 | 0.575 | 0.333 |
| dice | 0.769 | 0.714 | 0.500 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 1 | 0 | 0.955 | 0.800 | 1.000 | 0.800 | 0.889 |
| Pedestrian | 2 | 0 | 2 | 0.909 | 1.000 | 0.500 | 0.500 | 0.667 |
| ThreeWheeler | 8 | 3 | 1 | 0.818 | 0.727 | 0.889 | 0.667 | 0.800 |
| Truck | 1 | 0 | 2 | 0.909 | 1.000 | 0.333 | 0.333 | 0.500 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_060000.jpg | 7 | 1 | 3 | 0.636 | 0.875 | 0.700 |
| adasind_086220.jpg | 5 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_102750.jpg | 3 | 3 | 2 | 0.500 | 0.500 | 0.600 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|
| Bike | 4 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 2 | 0 | 0 | 2 |
| ThreeWheeler | 0 | 0 | 8 | 0 | 1 |
| Truck | 0 | 0 | 2 | 1 | 0 |
| <extra> | 1 | 0 | 1 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
