# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): (1) vạch chéo to ở giữa-dưới ảnh, từ khoảng (405, 651)
  xuống mép dưới (525, 719); (2) vạch chéo to ở dưới-phải, từ khoảng (697, 622) tới mép phải (957, 683). Hai vạch
  thuộc dãy ô gần camera, song song nhau, mỗi vạch ngăn hai ô đỗ cạnh nhau. Tôi vẽ thêm các vạch chéo ngắn của dãy ô
  giữa (hai nửa trên/dưới vạch cuối chung) và dãy ô xa theo cùng vai trò chia ô; mỗi polyline dừng ở chỗ phần sơn
  kết thúc.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: vạch mảnh dài chạy ngang gần hết bãi, khoảng từ (2, 543) tới
  (958, 507). Phóng to thấy các vạch chéo cắt qua nó từ cả hai phía, nên đây là vạch cuối chung của hai dãy ô quay đầu
  vào nhau, không phải đoạn sơn ngăn hai ô cạnh nhau. Bản export đầu tôi đã vẽ nó là `parking_line`, sau khi soát lại
  thì xoá. Mảng sáng loang ở đáy giữa ảnh (khoảng x 340–380, y ~710) cũng không vẽ: là vết bẩn/nhạt màu, không phải
  sơn kẻ ô.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: polygon chính phủ dải nhựa trống giữa dãy ô giữa và dãy
  ô gần — cạnh trên dừng ở đầu dưới các vạch chéo dãy giữa (y ~530–575), cạnh dưới dừng trước đầu trên các vạch to
  dãy gần (y ~587–676), hai bên kéo tới mép ảnh; không phủ lên ô đỗ và không cắt vạch nào. Hai polygon mảnh phía xa
  (y ~488–520) phủ lối đi giữa dãy ô xa và dãy ô giữa. Không có vật che trong các polygon; xe đỏ ở xa nằm ngoài
  polygon. Đây là vùng trống nhìn thấy trên một ảnh tĩnh, không phải kết luận vùng lái an toàn.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): (a) các vạch dãy xa (sát xe đỏ) nhỏ, mờ do ảnh cháy
  sáng, vị trí đầu/cuối vạch có thể lệch vài pixel; (b) vạch đứng ở góc dưới-trái (x ~24–27) và vạch ở mép phải
  (x ~923–959, y ~600) bị khung ảnh cắt, chỉ thấy một phần nên không chắc chia ô nào; (c) polygon mảnh phía xa chồng
  1–3 px lên đầu dưới vài vạch dãy xa và hai polygon mảnh chồng nhau một đoạn giữa ảnh — hạn chế độ chính xác khi vẽ ở
  khoảng cách xa; (d) vạch cuối chung đã loại có nên tính là ranh ô đỗ hay không, nếu guideline muốn gồm cả vạch cuối ô.
