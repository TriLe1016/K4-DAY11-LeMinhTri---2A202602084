# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | BOX_GEOMETRY | 1 |
| center | B2 | MISSING | 9 |
| center | B2 | SPURIOUS | 13 |
| center | B2 | WRONG_CLASS | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B2 | ATTRIBUTE | 1 |
| edge | B2 | MISSING | 2 |
| edge | B2 | SPURIOUS | 1 |
| edge | B2 | WRONG_CLASS | 2 |
| edge | C0 | BOX_GEOMETRY | 1 |
| mid | B2 | ATTRIBUTE | 2 |
| mid | B2 | BOX_GEOMETRY | 1 |
| mid | B2 | MISSING | 7 |
| mid | B2 | SPURIOUS | 10 |
| mid | C0 | BOX_GEOMETRY | 2 |
| unknown | B2 | MISSING | 2 |
| unknown | C0 | IGNORE_SCOPE | 1 |

## Top defects
- SPURIOUS: 25 (ví dụ frame adasind_019560.jpg)
- MISSING: 20 (ví dụ frame adasind_060000.jpg)
- BOX_GEOMETRY: 5 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: SPURIOUS (25) và MISSING (20) đứng đầu nhưng phần lớn không
  phải lỗi của người gán nhãn. Trong 25 SPURIOUS có 20 dòng `M_only` của model: lệch taxonomy (`E4_model_domain`) — model
  không có `ThreeWheeler` nên gán Car/Truck, thường 2 box trùng cho một xe (060000 M9+M10 trên R1; 086220 M5+M6 trên R1),
  và tách rider thành Pedestrian + Bike trái R03 (060000 M2/M3). Lỗi thật của tôi (`E1_annotator_error`) tập trung ở hai
  kiểu: (1) bỏ sót vật nhỏ/bị che trong cụm đông ở center frame 060000 (R5 người váy đỏ, R7 người cao 42 px, R9 xe ba
  bánh bị che — R01); (2) nhầm xe tải nhỏ ở xa thành xe ba bánh ở 102750 (L4, L6 — R04). Còn 102750 L5 là SPURIOUS
  nhưng tôi cho là reference thiếu (`E0_reference_defect`); 060000 L5 là khoảng trống luật về vùng unreadable
  (`E2_guideline_gap`).
- Cách sửa và ai nhận việc (`owner`): annotator (tôi) rework 6 ca P1/P2 ở P5 — thêm R5, R7, R9 ở 060000; đổi L4, L6
  sang Truck và sửa truncated/occluded của L1, L4 ở 102750 — và soát lại mỗi cụm đông bằng zoom trước khi khóa. `qa`
  xác nhận reference thiếu (Ticket 1). `ai_team` thêm class/ánh xạ ThreeWheeler và gộp rider trước khi dùng model
  pre-label (Ticket 2). `guideline` làm rõ R06 cho cụm vật mờ (20_guideline_patch.md).
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `screenshots/p4_ticket1_102750_ref_missing_L5.png`,
  `screenshots/p4_ticket2_060000_model_threewheeler.png`; findings `r1_craft` R5/R7/R9/L4/L6/L5 và `r3_diag` M9/M10/M12;
  `r3_diag/local_quality.md` (Truck recall 0.333 do 2 box Truck bị gán ThreeWheeler; Pedestrian recall 0.500 do thiếu
  2 người ở 060000); rule R01, R03, R04, R06.
