# Rework delta

| zone | matched before | matched after | missing before | missing after | spurious before | spurious after |
|---|---:|---:|---:|---:|---:|---:|
| center | 6 | 6 | 3 | 3 | 3 | 3 |
| mid | 7 | 7 | 1 | 1 | 0 | 0 |
| edge | 2 | 2 | 1 | 1 | 1 | 1 |

## Findings action=rework
- adasind_102750.jpg L1 ATTRIBUTE: đã sửa
- adasind_102750.jpg L1 BOX_GEOMETRY: đã sửa
- adasind_102750.jpg L4 WRONG_CLASS: chưa sửa
- adasind_102750.jpg L4 ATTRIBUTE: đã sửa
- adasind_060000.jpg NEW x13-45 y868-958 MISSING: không áp dụng
- adasind_060000.jpg R5 MISSING: chưa sửa
- adasind_060000.jpg R7 MISSING: chưa sửa
- adasind_060000.jpg R9 MISSING: chưa sửa
- adasind_102750.jpg L1+R1 ATTRIBUTE: đã sửa
- adasind_102750.jpg L4+R2 WRONG_CLASS: chưa sửa
- adasind_102750.jpg L6+R5 WRONG_CLASS: chưa sửa
- adasind_060000.jpg R5+M4 MISSING: chưa sửa
- adasind_060000.jpg R7+M6 MISSING: chưa sửa
- adasind_060000.jpg R9 MISSING: chưa sửa
- adasind_102750.jpg L4 SPURIOUS: đã sửa
- adasind_102750.jpg L6 SPURIOUS: đã sửa
- adasind_102750.jpg R2+M2 MISSING: chưa sửa
- adasind_102750.jpg R5+M8 MISSING: chưa sửa

## Đối chiếu thủ công (người làm bài)

Rework thực hiện theo `degrade rework` (mode.json): chỉ sửa **một** ca, khóa `rework` mã `0667-1BE0`. So từng box giữa
bản khóa `r1_craft` (3C78-0F20) và `rework` (0667-1BE0), thay đổi thật duy nhất là:

- `adasind_102750.jpg` L1 Truck: `truncated` false → **true**, `occluded` true → **false**; cạnh trái 825.35 → 827.62
  (chỉ lệch ~2 px). Căn cứ: box chạm cạnh phải khung x=1080 (R05), reference R1 truncated=true/occluded=false, QA r2_qa
  đã nêu trước khi mở reference.

Vì sao số theo zone **không đổi**: L1 vốn đã ghép với R1 ở IoU ≥ 0.5 (matched), nên sửa attribute không làm đổi
matched/missing/spurious; bảng zone chỉ đếm hình học + class, không đếm attribute. Lỗi ATTRIBUTE của L1 được sửa thật
nhưng không hiện trong bảng số.

Cột trạng thái tự động ở trên **không chính xác** với các dòng sau (tool đoán theo dòng liên quan cùng frame): L4
ATTRIBUTE, L4 SPURIOUS, L6 SPURIOUS và L1 BOX_GEOMETRY **chưa sửa** — L4 vẫn `ThreeWheeler`, `truncated=false`; L6 vẫn
`ThreeWheeler`; box L1 vẫn thừa ~27 px trái và ~35 px trên. Các ca rework còn lại (060000 R5, R7, R9 thiếu box;
102750 L4, L6 sai class) chưa làm trong buổi do hết thời gian; giữ `action=rework` trong findings để làm ở vòng sau.
Số trong bảng không bị sửa tay.
