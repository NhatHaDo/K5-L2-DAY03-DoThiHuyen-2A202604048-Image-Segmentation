# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

- Mã học viên theo lớp: 2A202604048 (Đỗ Thị Huyền)
- Ngày / CVAT local: 12/11/2026 / dùng CVAT online (app.cvat.ai) thay cho CVAT local
- Công cụ đã dùng: Polygon, Brush (chế độ polygon +/−). Không dùng SAM hay gợi ý tự động.

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | submissions/easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | submissions/medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | submissions/hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | submissions/cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | submissions/cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | submissions/cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | submissions/cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | submissions/cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | submissions/cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Sáu checkpoint và hard_panoptic làm bổ sung sau 12:00 theo sự đồng ý của coach. Không có lỗi export.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000181542.jpg`, người phụ nữ mặc áo dài đi giữa vạch qua đường, chính giữa ảnh.
- Class và quy tắc tôi dùng để chọn biên: `person`. Bám sát phần nhìn thấy: gồm tóc, tà áo dài và bàn chân; không lấn sang xe máy phía sau. Người và xe máy chồng nhau thì tách thành 2 object riêng.
- Nếu dùng gợi ý sau đó: không dùng.
- Nếu không dùng gợi ý: không dùng. Quyết định gán nhãn: chiếc xe máy phía sau bị người áo dài che thành 2 mảng nhìn thấy rời nhau, tôi gộp cả 2 mảng thành 1 instance `motorcycle` (theo phiếu quy tắc: vật bị che vẫn là một instance), không vẽ phần bị che.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `medium_instance` / `000000181542.jpg` / xe máy bên trái, phía sau người áo dài.
- Lỗi thuộc loại: sai lớp + gộp-tách (một vật bị thiếu mảng).
- Bằng chứng tôi nhìn thấy: nhãn trên canvas hiện "BICYCLE" cho chiếc xe máy; mask Polygon chỉ phủ mảng bên trái, bánh trước ở bên phải người áo dài không có mask.
- Quy tắc và hành động sửa: class phải khớp vật thật (xe máy → `motorcycle`); vật bị che vẫn là 1 instance. Xóa polygon cũ, vẽ lại bằng Brush → polygon (+) với label `motorcycle`, gộp cả 2 mảng nhìn thấy vào một object mask.
- Sau sửa đã Save và export lại chưa? Có.

Tự đánh giá GitHub Actions: chưa có điểm.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `000000181542.jpg`, xe bán tải giữa ảnh (Medium) | `car` hoặc `truck` | classes.json và phiếu quy tắc không định nghĩa; xe có thùng hàng hở phía sau; ảnh và taxonomy lấy từ COCO, COCO gán pickup là truck | Chọn `truck` |
| 2. `000000373353.jpg`, van trắng đậu góc trái (Medium) | `car` hoặc `truck` | Van chở người, không có thùng hàng; theo quy ước COCO thường là car | Chọn `car` |
| 3. `7ee6d192-89e2408b.jpg`, sườn đồi cỏ khô và bụi cây hai bên cao tốc (Easy) | `vegetation` hoặc để trống (không có class terrain) | Task chỉ có 5 class; vùng có bụi cây và thảm cỏ | Chọn `vegetation`. Câu hỏi cho coach: nếu reference để trống vùng đồi cỏ thì nên xử lý thế nào? Xe và dải phân cách để trống vì không có class. |
| 4. `000000144300.jpg` (cp1_holes), caravan trắng bên trái | `car`/`truck` hoặc để trống | Không có class caravan trong classes.json; caravan không phải xe tự chạy | Xóa object, để trống vùng caravan |
