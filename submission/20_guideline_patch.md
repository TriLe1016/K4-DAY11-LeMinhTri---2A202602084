# Guideline patch

- **Rule mới đề xuất:** **R06a — Cụm vật không tách được.** Khi ≥2 vật động (người, xe hai bánh) chồng lên nhau ở xa
  mà không xác định được biên từng vật (không thấy rõ đầu–chân hoặc bánh xe của từng vật), vẽ **một** polygon
  `ignore_region` `reason=unreadable` (hoặc `crowd_or_group` nếu đếm được ≥3 vật) bao cả cụm và **không** box từng vật
  bên trong. Nếu tách được biên một vật cao ≥ 40 px thì box vật đó và cắt vật đó ra khỏi polygon. Mọi box nằm ≥ 30 %
  trong polygon unreadable phải được xem lại trước khi khóa.
  *Ví dụ:* `adasind_060000.jpg`, cụm người/xe máy mờ ở x~264–294 y~871–918: reference vẽ `unreadable`, tôi vẽ box `Bike`
  L5 x278–308 y878–922 chồng ~48 % — dưới ngưỡng 50 % của R09 nên bị tính SPURIOUS thay vì IGNORE_SCOPE, dù hai bên
  thực chất đang bất đồng về một câu hỏi luật chưa trả lời.
- **Áp dụng cho:** `ignore_region.reason` = `unreadable` / `crowd_or_group`; class `Pedestrian`, `Bike`; chủ yếu zone
  center/mid nơi dòng xe đông ở xa (tập con 6 class động của lab, không phải taxonomy ADASIND đầy đủ).
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R06 chỉ liệt kê tên các reason, không nói *khi nào* một
  cụm vật mờ là `unreadable` thay vì box từng vật; R09 chỉ loại box nằm ≥ 50 % trong ignore, nên box chồng 30–50 % rơi
  vào vùng xám và cho kết quả khác nhau giữa annotator, QA và reference.
- **`rules_version` mới:** v1.0.0 → v1.1.0
- **Hiệu lực từ:** round `rework` sau khi Lab Coach duyệt; các bản đã khóa trước đó giữ nguyên, finding liên quan ghi
  `rules_version=v1.0.0`.
