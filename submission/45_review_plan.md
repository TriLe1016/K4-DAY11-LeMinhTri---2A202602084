# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Dòng xe đông ở center/mid, ví dụ `adasind_060000.jpg` | L thiếu 3 vật (R5, R7, R9 — MISSING, E1), 1 box chồng vùng unreadable (L5, E2); model 7 box `M_only` ở frame này | Frame có nhiều vật nhỏ, bị che, đứng sát nhau nhất; recall Pedestrian của tôi chỉ 0.500; đây là nơi rule R01/R06 dễ bị hiểu khác nhau | Crop zoom từng cụm, chiều cao đo được của vật sát ngưỡng 40 px, polygon unreadable của reference, findings `r1_craft` R5/R7/R9/L5 |
| Xe tải nhỏ / xe ba bánh ở xa và ở edge, ví dụ `adasind_102750.jpg` | 2 WRONG_CLASS (L4, L6: ThreeWheeler → Truck), 2 ATTRIBUTE truncated (L1, L4), 1 ca nghi reference thiếu (L5, E0) | Lỗi class làm Truck recall còn 0.333; vật ở edge bị khung cắt cần kiểm `truncated`; frame ngược sáng khiến model sinh box khổng lồ (M5) | Screenshot Ticket 1, danh sách K12 của lệnh `cvat`, cặp box L/R/M, quyết định của `qa` cho Ticket 1 |

Giới hạn của kết luận từ ba frame ADASIND: chỉ 3 frame, 20 vật reference, edge chỉ có 3 vật; một camera trước của xe
hai bánh, cùng một đoạn đường và điều kiện nắng; teaching reference có thể sai (Ticket 1). Các con số chỉ chỉ ra *chỗ
nên soi*, không đo được tỷ lệ lỗi thật, không so được giữa zone, và không suy ra được cho các camera sau/trái/phải.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi: mỗi ô camera × normal/hard
được lấy mẫu phân tầng theo **clip/chuyến đi** trước rồi mới đến frame: một clip đóng góp tối đa 2 frame, hai frame
trong cùng clip phải cách nhau ≥ 5 giây, để 30 frame liền nhau của cùng một ngã tư không bị đếm như 30 ca độc lập. Độ phủ
được kiểm bằng bảng đếm theo thời điểm (ngày/đêm/ngược sáng), loại cảnh (bãi đỗ, đường đông, lề có curb) và loại vật
(xe ba bánh, xe tải nhỏ, người sát xe) cho từng camera; ô nào thiếu một loại thì bổ sung trước khi review. Ca hard lấy
từ đúng kiểu lỗi đã thấy ở đây — cụm đông ở xa, xe tải nhỏ vs xe ba bánh, vật bị khung cắt, lóa nắng — nhưng phải tìm
lại trên từng camera vì góc lắp và ego_body khác nhau. Vì các ô hard được chọn có chủ đích (không ngẫu nhiên) và 200
frame chia 8 ô chỉ còn 20–35 frame/ô, kế hoạch này giúp **tìm và phân loại lỗi**, không cho phép ước lượng tỷ lệ lỗi
của 50.000 frame; muốn đo tỷ lệ cần thêm một mẫu ngẫu nhiên riêng không lọc theo độ khó.
