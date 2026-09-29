# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao? **Không phải `DUPLICATE`, cần quy tắc riêng.** `DUPLICATE` là hai box cho một vật *trong cùng
   một ảnh*. Ở seam, mỗi camera là một ảnh riêng; vật thật sự xuất hiện trên cả hai, nên mỗi ảnh cần box của mình
   (bám phần nhìn thấy trên ảnh fisheye gốc, có thể `truncated` ở một bên và méo ở bên kia). Việc có hợp nhất thành
   một object hay không là câu hỏi của tầng sau và phụ thuộc policy output (hiển thị BEV, đầu vào fusion, hay đánh giá
   theo từng camera), timestamp đồng bộ và calibration để chứng minh hai box là cùng một vật. Quy tắc riêng cần nói: giữ
   box ở mỗi camera, đánh dấu ca seam, và chỉ ghép khi đủ ba điều kiện đó.
2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera. **Giữ cùng track ID** khi đó vẫn là cùng một vật
   quan sát được liên tục, kể cả khi bị che ngắn rồi xuất hiện lại ở vị trí hợp lý. **Thêm keyframe** khi hình học đổi
   đáng kể — vật đi từ center ra edge và bị méo/cắt, đổi hướng, hoặc attribute đổi (`occluded`, `truncated`) — để nội
   suy không kéo box lệch. **Đặt Outside** khi vật rời trường nhìn hoặc bị che hoàn toàn, thay vì để box nội suy trôi
   qua frame không còn thấy vật; nếu vật quay lại mà không chắc là cùng vật thì mở track mới. Trước khi nối track qua
   hai camera cần: timestamp đồng bộ giữa hai camera, calibration intrinsics + extrinsics để chiếu hai vị trí về cùng
   hệ tọa độ và thấy chúng trùng trong dung sai, và policy output nói rõ có cần một ID xuyên camera hay không.
3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm? Ở `adasind_102750.jpg` `L5` tôi box một
   tuk-tuk cao 54 px trên lề phải; reference không có box nên `compare` tính là SPURIOUS. Tôi không xóa box theo
   reference mà zoom lại ảnh gốc, thấy vật rõ, model (`M6`) cũng phát hiện vật ở cùng chỗ, rồi ghi
   `E0_reference_defect`, `action=escalate` (Ticket 1, decision log D05) và giữ box khi rework. Nếu làm lại slice
   này, tôi sẽ zoom từng cụm đông trước khi khóa để không bỏ sót vật nhỏ (060000 R5, R7, R9), kiểm class xe tải nhỏ vs
   xe ba bánh bằng hình dáng thùng hàng (102750 L4, L6), và soát `truncated` cho mọi box chạm cạnh khung.
