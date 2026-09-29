# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Người băng qua sát mũi xe bị vòng kính cắt; ngược sáng/lóa nắng; xe tải nhỏ và xe ba bánh ở xa lẫn nhau | Lóa làm model sinh box giả (ADASIND 102750 M5); vật ở xa dễ nhầm class (102750 L4/L6 ThreeWheeler vs Truck); rìa méo dễ quên `truncated` | Ảnh fisheye gốc (không undistort), tâm + bán kính vòng kính, intrinsics front, timestamp; lens_border/ego_body đo riêng cho camera này | 2 annotator độc lập gán nhãn mù; QA thứ ba phân xử mọi cặp IoU < 0.7 hoặc khác class/attribute theo rule; chỉ nhận vào gold khi hai người + QA thống nhất và ghi decision log |
| rear | Người/trẻ em/xe hai bánh sát đuôi khi lùi; ban đêm; ống kính bẩn mưa/bùn | Vật rất gần, lớn, bị cắt bởi vòng kính và thân xe; ban đêm và bụi bẩn làm vật mờ → dễ nhầm unreadable vs box (khoảng trống R06 trong `20_guideline_patch.md`) | Ảnh gốc rear, intrinsics + extrinsics rear, polygon ego_body (cản sau) riêng, timestamp, trạng thái số lùi nếu có | Như front, thêm soát riêng mọi polygon ignore; ca đêm/bẩn cần người thứ ba đo chiều cao phần thấy so với H=40 trước khi quyết box hay ignore |
| left | Seam góc trước-trái và sau-trái; người mở cửa, xe máy vượt sát hông; curb sát bánh | Cùng vật xuất hiện ở hai camera với hai box khác nhau; gương và sườn xe (ego_body) chiếm nhiều khung; vật méo mạnh ở rìa | Ảnh gốc left, extrinsics left để ghép với front/rear, timestamp đồng bộ, polygon ego_body gương/sườn trái | Review từng camera độc lập trước; ca seam đánh dấu riêng, không ghép box hay xóa box khi chưa có policy cross-camera; QA kiểm cả hai ảnh cùng timestamp |
| right | Seam góc trước-phải và sau-phải; người trên vỉa hè sát xe; cột/curb che khuất | Như left; ngoài ra vật bị cột che dễ gán sai `occluded`/`truncated` (lỗi đã gặp ở 102750 L1) | Ảnh gốc right, extrinsics right, timestamp đồng bộ, polygon ego_body gương/sườn phải | Như left; thêm checklist attribute truncated/occluded vì đây là lỗi lặp lại của annotator |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): khi thay module camera hoặc ống kính (vòng kính và
  ego_body đổi), khi calibration intrinsics/extrinsics đổi quá dung sai hoặc camera bị lắp lệch, khi rule đổi phiên bản
  (ví dụ v1.0.0 → v1.1.0 cho R06a) — các frame bị ảnh hưởng bởi rule mới phải được gán lại và review lại; và định kỳ
  khi dữ liệu mới có miền khác (mùa mưa, ban đêm, thành phố mới). Teaching reference ADASIND hiện tại không dùng làm
  gold: nó chỉ có một camera, vài frame, do một người sửa tay và đã thấy ít nhất một vật bị thiếu (Ticket 1).
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: một xe máy vượt qua góc trước-trái xuất hiện
  cùng lúc ở rìa ảnh front (bị vòng kính cắt, `truncated=true`) và ở rìa ảnh left (méo mạnh). Trong từng camera, cả
  hai box đều hợp lệ và không phải `DUPLICATE`. Chỉ được gán cùng track ID hoặc hợp nhất khi có: timestamp hai camera
  đồng bộ trong dung sai, calibration extrinsics để chiếu hai box lên cùng hệ tọa độ (BEV) và kiểm vị trí trùng, và
  policy output cho biết hệ thống đích cần một object hợp nhất hay giữ nhãn theo từng camera. Thiếu một trong ba thì giữ
  hai box riêng và ghi ca vào decision log.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:
  peer agreement chỉ cho biết hai người hiểu luật giống nhau — cả hai có thể cùng sai (tôi và reference cùng không box
  tuk-tuk sau gốc cây ở 086220). Quality report ở đây đo độ khớp với teaching reference trên 3 frame của **một**
  camera trước, cùng góc lắp, cùng ego_body và điều kiện nắng; nó không chứa seam, không có camera sau/hông, không có
  calibration hay timestamp để kiểm ca cross-camera. Gold set bốn camera cần review độc lập theo từng camera, có người
  phân xử và có policy cho seam, trước khi gọi là gold.
