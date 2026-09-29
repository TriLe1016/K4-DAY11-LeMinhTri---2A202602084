# Escalation ticket

## Ticket 1

- **Frame:** `adasind_102750.jpg`, vật `L5` — tuk-tuk trên lề phải, box L x413–462 y926–980 (cao 54 px ≥ H=40).
  Ca liên quan cần đo lại: `adasind_086220.jpg` model `M7` x38–100 y964–1010 (tuk-tuk đỗ sau gốc cây, L và R đều
  không box).
- **Ảnh chụp:** `submission/screenshots/p4_ticket1_102750_ref_missing_L5.png` (box L5 xanh, box model M6 đỏ).
- **Expected impact:** teaching reference B2-mid thiếu ít nhất một vật trong phạm vi R01, nên `compare` và
  `local-quality` tính box đúng của annotator là SPURIOUS/FP (102750 precision 0.500). Nếu reference này được dùng làm
  mốc cho người khác hoặc cho model, lỗi thiếu sẽ bị lan sang mọi phép đo trên slice. Findings liên quan:
  `r1_craft`/`r3_diag` `L5` (E0_reference_defect) và `r3_diag` `M6`, `M7`.
- **Owner:** `qa` (người duy trì teaching reference).
- **Recommendation:** người soát reference mở ảnh gốc 102750, xác nhận vật là `ThreeWheeler` cao ≥ 40 px và bổ sung
  box vào reference (hoặc ghi rõ lý do loại). Với 086220 M7, đo phần nhìn thấy sau gốc cây: ≥ 40 px thì bổ sung
  `ThreeWheeler` `occluded=true` cho cả reference và bài làm, < 40 px thì đóng ticket. Tôi giữ nguyên L5 trong bài
  (không xóa theo reference) cho tới khi có quyết định.

## Ticket 2

- **Frame:** cả ba frame B2-mid; ví dụ rõ nhất `adasind_060000.jpg` R1 (xe ba bánh x347–418) bị model gán đồng thời
  `M9 Truck` và `M10 Car`, R8 bị gán `M12 Car`.
- **Ảnh chụp:** `submission/screenshots/p4_ticket2_060000_model_threewheeler.png`.
- **Expected impact:** model YOLO26m đóng băng không có class `ThreeWheeler`: 9/10 xe ba bánh trong reference hoặc
  bị gán Car/Truck (thường hai box trùng) hoặc không có box (060000 R3, R4; 086220 R5). Model cũng tách rider thành
  `Pedestrian` + `Bike` trái với R03. Nếu dùng model này để pre-label, annotator phải sửa class gần như mọi xe ba bánh
  và xóa box trùng; số đo model ở zone mid (4 missing, 10 thừa) phần lớn là lệch taxonomy chứ chưa chứng minh model
  kém ở vùng méo fisheye.
- **Owner:** `ai_team`.
- **Recommendation:** thêm class `ThreeWheeler` (hoặc bảng ánh xạ sau suy luận có kiểm tra) và gộp person + motorcycle
  chồng nhau thành một `Bike` trước khi dùng làm pre-label; chạy lại so sánh trên nhiều slice hơn trước khi kết luận
  `E4_model_domain` do méo. Riêng box khổng lồ `M5` ở 102750 trùng vùng lóa nắng — thêm frame ngược sáng vào tập hard.
