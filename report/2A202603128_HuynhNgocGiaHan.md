# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603128
- Ngày / CVAT local: 12/11/2026
- Công cụ đã dùng: CVAT polygon vẽ tay (easy_semantic, medium_instance, một phần hard_panoptic); SegFormer-b2 và Segment Anything 3 có trợ lý AI Claude hỗ trợ chạy (phần còn lại của hard_panoptic và các checkpoint).

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip |  3/ 3 | 20 |
| medium_instance | medium_instance.zip|  3/ 3 | 32 |
| hard_panoptic | hard_panoptic .zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin.zip | cp3_thin.zip  | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: 000000181542.jpg (ảnh đường phố có nhiều xe máy), giữa ảnh, người phụ nữ mặc áo dài đang qua đường.
- Class và quy tắc tôi dùng để chọn biên: `person`. Tôi tô toàn bộ người từ tóc đến dép, gồm cả túi xách đang đeo vì là đồ mang trên người. Polygon dừng ở mép tà áo, không tô bóng đổ trên mặt đường. Người này đứng trước xe máy nên polygon của người không bị cắt.
- Nếu không dùng gợi ý: không dùng. Ở medium_instance tôi vẽ polygon tay cho tất cả object. Một quyết định gán nhãn của tôi: xe máy bên trái bị người phụ nữ mặc áo dài che ngang giữa thân, bánh trước lộ ra ở bên phải người. Tôi coi hai phần là cùng một `motorcycle` và không vẽ xuyên qua người, vì vật bị che vẫn là một instance.
## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: medium_instance / 000000181542.jpg / bánh xe trước nằm ngay bên phải người phụ nữ mặc áo dài, ở giữa ảnh.
- Lỗi thuộc loại: sai lớp.
- Bằng chứng tôi nhìn thấy: Bánh xe này đang mang nhãn `bicycle`, nhưng trong ảnh không có chiếc xe đạp nào. Bánh nằm thẳng hàng với thân chiếc xe máy bên trái, phần giữa của xe bị người phụ nữ che mất, nên đây là bánh trước của chính chiếc xe máy đó.
- Quy tắc và hành động sửa: Vật bị che ở giữa vẫn là một instance và mang cùng một class. Tôi đổi nhãn bánh xe từ `bicycle` sang `motorcycle`. Khi soát lại vùng này tôi cũng thấy polygon `person` của người lái áo sáng bên phải trùm cả phần yếm trắng của xe, nên tôi thu polygon lại, chỉ giữ thân, tay và chân người; phần yếm trắng thuộc `motorcycle`.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại sau khi sửa.
  
Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): … / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.


| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. 000000144300.jpg, xe caravan trắng bên trái, phía sau xe máy Honda | Gán `car` vì là xe chở người, hoặc để trống vì là rơ-moóc không tự chạy (gợi ý tự động đề xuất `truck`) | Quy tắc của tôi: xe có chức năng chính là chở người thì là `car`, chở hàng thì là `truck`. Caravan là chỗ ở cho người, không chở hàng | Tôi gán `car`. Câu hỏi: caravan/rơ-moóc nên là `car` hay để trống? |
| 2. 000000181542.jpg, xe bán tải màu bạc ở giữa ảnh, phía sau người mặc áo dài | Gán `truck` vì có thùng chở hàng phía sau, hoặc `car` vì cabin chở người | Xe có thùng hở phía sau là dấu hiệu chở hàng, nhưng cabin 4 cửa giống xe con. Quy tắc chở người/chở hàng của tôi không phân định rõ ca này | Tôi gán `truck`. Câu hỏi: xe bán tải thuộc class nào theo quy ước của lớp? |
| 3. 000000460147.jpg (hard_panoptic), xe đầu kéo chở ô tô ở làn giữa | Các ô tô đang được chở là từng `car` riêng, hoặc là hàng hoá nên thuộc luôn mask `truck` | Từng ô tô vẫn nhìn rõ và đếm được; đề yêu cầu một mask không thay cho mọi xe | Tôi tách mỗi ô tô thành một `car` riêng nằm lớp trên `truck`. Câu hỏi: cách tách này có đúng không? |
