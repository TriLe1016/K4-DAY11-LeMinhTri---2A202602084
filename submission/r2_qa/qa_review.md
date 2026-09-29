# QA review · B2-mid

Mã khóa: 3C78-0F20

**Hình thức:** cold review. Nhóm đổi bài theo vòng (tri soát vu · B3-dense), nhưng chờ quá 5 phút chưa nhận được file
đã khóa của Vũ, nên theo `docs/03-roles-rotation-vi.md` tôi nghỉ rồi tự soát lại bản khóa của chính mình. Chỉ dùng
ảnh gốc, `qa_overlay.html` và `docs/02-rules-vi.md`; chưa mở teaching reference, model overlay hay worked HTML.
`object_ref` = thứ tự box trong XML của frame (L1, L2, …); vật chưa có box ghi `NEW` kèm tọa độ ước lượng.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_102750.jpg | L1 | R05 | Truck đỏ bên phải bị cạnh phải khung ảnh cắt (box chạm x=1080) nhưng `truncated=false`; không thấy vật nào che phía trước mà lại `occluded=true`. Hai attribute đang bị đảo. |
| adasind_102750.jpg | L1 | R02 | Box rộng hơn phần xe nhìn thấy ~35 px bên trái và ~30 px phía trên, ăn cả cột điện và nền. |
| adasind_102750.jpg | L4 | R04 | Vật sát mép trái (x 0–87) có cabin và thùng hàng hình hộp, không thấy dáng xe ba bánh; gán `ThreeWheeler` có vẻ sai, cần xem là `Truck`. |
| adasind_102750.jpg | L4 | R05 | Box chạm cạnh trái khung (x=0), vật bị cắt nhưng `truncated=false`. |
| adasind_102750.jpg | L6 | R04 | Vật nhỏ ở xa (x 325–379, h=44) có đuôi thùng hình hộp giống xe tải; class `ThreeWheeler` cần soát lại khi zoom. |
| adasind_102750.jpg | NEW (x~253–333, y~887–943) | R01 | 1–2 xe tải xanh/xám phía sau L2, bị che một phần, phần thấy cao ~45–55 px (≥ H=40) nhưng chưa có box. |
| adasind_060000.jpg | L2 | R02 | Box prefill xe điện xanh-trắng: cạnh dưới y=1377 cắt ngang bánh trước (bánh xuống tới ~y 1400); cạnh trái thừa ~25 px. Prefill được giữ nguyên, chưa sửa hình học. |
| adasind_060000.jpg | NEW (x~13–45, y~868–958) | R01 | Người váy đỏ cầm ô ở mép trái, cao ~90 px, đứng cạnh hai người L7/L8 nhưng chưa có box `Pedestrian`. |
| adasind_086220.jpg | NEW (x~38–98, y~963–1012) | R01 | Tuk-tuk vàng-trắng đỗ sau gốc cây bên trái, phần thấy cao ~49 px, chưa có box (nếu vẽ: `ThreeWheeler`, `occluded`). |

**Đã kiểm, không thấy vấn đề:** rider + xe là một box `Bike` bao cả đầu người lái (060000 L3, 086220 L1, L5 — R03);
mỗi frame đủ 2 `lens_border` và 1 `ego_body` không bao bóng (R07, R08); mỗi `ignore_region` có một `reason` (R06);
không box nào nằm ≥50% trong vùng ignore (R09); không có box trùng.

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
