# Tự soát

- adasind_102750.jpg L1: truncated khác dự kiến
- adasind_102750.jpg L4: truncated khác dự kiến
- Tên task thiếu raw_fisheye

## Cảnh báo tự động và cách xử lý

Bản nháp đầu (`r1-draft.zip`) có 6 cảnh báo; bản khóa còn 3.

- **Đã xử lý:** thiếu `ego_body` ở cả 3 frame (060000, 086220, 102750) — đã vẽ polygon `ignore_region`,
  `reason=ego_body` bao tay/áo người lái ở mép trái-dưới, không bao bóng người lái trên mặt đường.
- **Chưa xử lý trước khi khóa — ghi nhận để rework:** 102750 L1 (Truck đỏ bên phải) bị cạnh phải khung ảnh cắt nên
  cần `truncated=true`; đang để `occluded=true` dù không có vật nào che phía trước. 102750 L4 (xe sát mép trái, x 0–87)
  chạm cạnh trái khung nên cần `truncated=true`.
- **Tên task thiếu raw_fisheye:** task CVAT tên `Day11 · ADASIND · B2-mid · raw_fisheye`, nhưng export từ job
  (`job_24_…zip`) không ghi tên task vào XML. Lần export sau dùng **Export task dataset**.

## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ — đo chiều cao các vật nhỏ ở xa: 060000 L5 Bike h=44, 086220 L3 Bike h=41,
  102750 L6 h=44 giữ box; xe máy/người ở xa dưới 40 px không vẽ (R01).
- [x] lens_border và ego_body — mỗi frame có 2 `lens_border` từ prefill (không xóa, không vẽ thêm, R08) và 1 `ego_body`
  (R07); cả 3 frame đều thấy thân người lái nên đều cần ego.
- [x] Class sáu nhãn — xe ba bánh chở người/hàng gán `ThreeWheeler` (R04), người trong xe không box riêng (R03). Còn
  nghi: 102750 L4 (x 0–87) và L6 (x 325–379) có thùng hình hộp giống xe tải hơn xe ba bánh; danh sách K12 của lệnh
  `cvat` ghi "Truck edge (45,906)" đúng vị trí L4 → nhiều khả năng phải là `Truck`.
- [x] Rider và Bike — người lái + xe máy/xe đạp là một box `Bike` bao cả đầu người lái: 060000 L3, 086220 L1 và L5 (R03).
- [x] Geometry trên ảnh fisheye gốc — box vẽ trên ảnh méo, không nắn thẳng (R02). Còn lệch: 060000 L2 (xe điện
  xanh-trắng, prefill) cạnh dưới y=1377 cắt ngang bánh trước (bánh tới ~y 1400); 102750 L1 rộng ~35 px trái và ~30 px
  trên.
- [x] truncated và occluded — 060000 L6 và 086220 L2 bị cạnh khung cắt đã `truncated=true` (R05). Chưa sửa 102750 L1
  và L4 (xem cảnh báo tự động ở trên).
- [x] Vật thiếu hoặc box trùng — không có box trùng. Nghi thiếu: 060000 người váy đỏ cầm ô ở mép trái (x ~13–45,
  y ~868–958, cao ~90 px); 086220 tuk-tuk đỗ sau gốc cây bên trái (x ~38–98, y ~963–1012, cao ~49 px); 102750 1–2 xe
  tải bị L2 che (x ~253–333, cao ~45–55 px).
- [x] ignore_region có reason — 9 polygon, mỗi cái đúng một `reason` (`lens_border` ×6, `ego_body` ×3, R06); không box
  nào nằm ≥50% trong vùng ignore (R09).
- [x] Tên task raw_fisheye và export CVAT 1.1 — task đặt đúng tên, export định dạng CVAT for images 1.1, không kèm ảnh;
  export job không mang tên task (xem trên).

Các ca "chưa sửa / nghi thiếu" giữ nguyên trong bản khóa (mã `3C78-0F20`) để đối chiếu ở P4 và sửa có căn cứ ở P5.
