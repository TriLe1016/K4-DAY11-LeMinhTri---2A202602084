# Sensor context

- Rig: ảnh dọc 1080×1920, một camera fisheye nhìn về phía trước theo hướng xe chạy. Trong khung thấy tay người lái
  (áo xanh) cầm tay lái, gương chiếu hậu bên trái và bóng người + xe in trên mặt đường, nên theo quan sát camera gắn
  trên/cùng người lái một xe hai bánh (ADASIND không kèm tài liệu rig; không suy ra độ cao, góc nghiêng hay
  calibration). Đây là **một** camera, không đại diện cho rig bốn camera front/rear/left/right của SVM.
- `ego_body`: khi nhìn thấy, nằm ở góc dưới bên trái vòng kính — tay áo xanh và bàn tay trên tay lái, cụm tay lái,
  gương, phần chân/đầu gối người lái; kéo từ khoảng giữa mép trái xuống đáy vòng kính. Không phải frame nào cũng thấy:
  có frame chỉ còn bóng tối/bóng đổ ở mép trái-dưới (ví dụ `adasind_006840.jpg`, `adasind_271039.jpg` không có thân xe
  nhìn thấy), nên chỉ vẽ `ego_body` khi thật sự thấy thân xe/người lái.
- Vòng kính: hình tròn tâm gần giữa khung (cx ≈ 420–670 px, cy ≈ 890–1080 px), bán kính ≈ 770–840 px
  (`assets/frames.csv`). Đường kính (~1.620 px) lớn hơn bề ngang ảnh nên vòng bị hai cạnh trái/phải khung cắt; phía
  trên và dưới vòng là vùng đen/vỏ ống kính. Vòng kính chiếm khoảng 73–81 % (trung bình ~77 %) diện tích khung hình;
  gần mép vòng ảnh méo mạnh (đường dây điện, vạch đường cong lại) và vật gần như xe ba bánh bên phải bị vòng kính cắt
  (`truncated`).
