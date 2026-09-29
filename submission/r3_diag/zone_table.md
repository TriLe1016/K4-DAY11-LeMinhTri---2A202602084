# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 9 | 3 | 3 | 5 | 8 | SPURIOUS (2) |
| mid | 8 | 1 | 0 | 4 | 10 | MISSING (1) |
| edge | 3 | 1 | 1 | 2 | 0 | WRONG_CLASS (1) |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: người (L) gãy nhiều nhất ở **center**
  (n_ref=9: 3 missing + 3 spurious). Cả 3 missing đều ở frame 060000 — vật nhỏ/bị che trong dòng xe đông (R5 người
  váy đỏ, R7 người cao 42 px, R9 xe ba bánh bị che); 3 spurious gồm 2 lỗi class (102750 L6) và 1 box ở rìa vùng
  unreadable (060000 L5), 1 ca nghi reference thiếu (102750 L5). Model (M) gãy nặng nhất ở **mid** (n_ref=8: 4 missing,
  10 box thừa) và center (5 missing, 8 thừa). Ở edge L chỉ có 1 missing + 1 spurious và đều là cùng một vật bị gán
  sai class (102750 L4 ThreeWheeler vs R2 Truck), không phải lỗi bỏ sót.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: lỗi của L chủ yếu
  là **class xe tải nhỏ/xe ba bánh ở xa** (R04) và **vật nhỏ bị che trong cụm đông** (R01), không phải do méo rìa —
  vùng edge L gần như khớp. Lỗi của M chủ yếu do **lệch taxonomy**: model không có class ThreeWheeler nên mọi xe ba
  bánh thành Car/Truck (thường hai box trùng cho một xe), và tách rider thành Pedestrian + Bike trái với R03; box
  model M1 trên thân người lái đã bị `ego_body` loại, cho thấy thiếu `ego_body` sẽ đẩy FP lên. Giới hạn: chỉ 3 frame,
  20 vật reference (edge chỉ 3 vật), một camera ADASIND; reference là teaching reference có thể thiếu (102750 L5), nên
  số theo zone chỉ gợi ý nơi cần soi, không đo được tỷ lệ lỗi hay kết luận méo fisheye gây lỗi.
