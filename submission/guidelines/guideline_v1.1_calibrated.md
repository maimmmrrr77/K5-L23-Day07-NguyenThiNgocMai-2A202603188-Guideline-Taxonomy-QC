# Hướng dẫn gán nhãn thống nhất — v1.1

- **Phạm vi:** bounding box 2D cho người và phương tiện giao thông trong ảnh đường phố.
- **Định dạng:** CVAT, xuất COCO 1.0.
- **Taxonomy áp dụng:** `submission/taxonomy.json` phiên bản `v1.0`.
- **Nguyên tắc:** chỉ gán theo bằng chứng nhìn thấy trong ảnh; không suy đoán phần bị che hoặc nằm ngoài khung hình.

## 1. Lớp

| Lớp             | Định nghĩa thống nhất                                                                                                                                  | Không bao gồm                                                                                         |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| `car`           | Ô tô chở người, gồm sedan, hatchback, coupe, minivan không thuộc định nghĩa `van`, SUV và crossover.                                                   | Xe tải/pickup chở hàng (`truck`), xe thân hộp kiểu van (`van`), xe buýt (`bus`).                      |
| `truck`         | Phương tiện cơ giới chở hàng có thùng hoặc sàn chở hàng riêng; gồm xe tải chuyên dụng và pickup có khoang chở hàng.                                    | Ô tô chở người, xe van thân liền khối, xe buýt.                                                       |
| `van`           | Phương tiện thân hộp cỡ nhỏ đến trung bình, khoang lái nối liền khoang kín chở hàng hoặc hành khách.                                                   | Xe tải có thùng/sàn riêng (`truck`), xe buýt chở khách quy mô lớn (`bus`).                            |
| `bus`           | Phương tiện chở khách có cấu hình xe buýt hoặc xe khách đường dài, được nhận diện qua hình dáng và công năng thể hiện trong ảnh.                       | Van chở khách cỡ nhỏ (`van`), ô tô chở người (`car`).                                                 |
| `tuk_tuk`       | Phương tiện cơ giới ba bánh dùng chở người hoặc hàng, gồm tuk-tuk, xe lam và xe lôi ba bánh có động cơ.                                                | Xe máy hai bánh (`motorcycle`), xe ba bánh không động cơ.                                             |
| `motorcycle`    | Phương tiện cơ giới hai bánh, gồm xe máy, mô tô, xe tay ga và xe gắn máy điện.                                                                         | Xe đạp không động cơ, tuk-tuk và phương tiện ba bánh.                                                 |
| `person`        | Người thật trong cảnh, gồm người đi bộ, người lái và hành khách nhìn thấy được.                                                                        | Tượng, ma-nơ-canh, hình in/ảnh trên biển quảng cáo hoặc người chỉ xuất hiện trong phản chiếu.         |
| `ignore_region` | Vùng nhỏ đánh dấu đối tượng thuộc phạm vi nhưng không thể gán riêng đáng tin cậy, như mục tiêu dưới ngưỡng kích thước hoặc cụm quá dày không thể tách. | Đối tượng ngoài taxonomy, vùng nền lớn, vật bị che hoàn toàn, hoặc cách thay thế cho một lớp chưa rõ. |

Chọn lớp theo cấu tạo và công năng có bằng chứng trực tiếp trong ảnh, không chỉ theo màu sắc hoặc kích thước ước đoán. Nếu vẫn không phân biệt được các lớp có định nghĩa gần nhau, chọn lớp có bằng chứng tốt nhất và bật `can_review`.

## 2. Quy tắc gán nhãn

### R-01 — Một đối tượng, một box; người và phương tiện tách riêng

Tạo một box riêng cho mỗi người hoặc phương tiện thuộc taxonomy khi có thể phân biệt được. Người lái/ngồi trên xe máy được gán box `person` và box `motorcycle` riêng; hai box có thể chồng lấn. Không gộp nhiều đối tượng vào một box. Chỉ dùng `ignore_region` cho phần mục tiêu không thể tách đáng tin cậy; vẫn gán riêng các đối tượng phân biệt được.

### R-02 — Box theo phần nhìn thấy

Vẽ box chữ nhật song song cạnh ảnh, ôm sát các điểm cực của phần đối tượng nhìn thấy. Không bao gồm bóng, khoảng trống, hoặc phần bị vật khác che mà không nhìn thấy; không vẽ xuyên qua vật che và không suy đoán đường bao bị khuất.

Gán `occluded_level` theo ước lượng tỷ lệ diện tích toàn vật bị vật khác che:

| Giá trị | Mức che khuất                                           |
| ------- | ------------------------------------------------------- |
| `0`     | Không bị vật khác che.                                  |
| `1`     | Bị che trên 0% và dưới 25%.                             |
| `2`     | Bị che từ 25% đến 50%, tính cả hai mốc.                 |
| `3`     | Bị che trên 50%, nhưng vẫn đủ bằng chứng nhận diện lớp. |

Không tính phần nằm ngoài khung ảnh vào mức che khuất. Nếu không đủ bằng chứng nhận diện lớp, không đoán; ghi nhận ca cần xem xét.

### R-03 — Vật bị cắt bởi mép ảnh

Đặt `truncated=yes` khi đối tượng tiếp tục vượt ra ngoài một cạnh khung ảnh. Box chỉ bao phần nằm trong ảnh và kết thúc đúng tại cạnh đó. Đặt `truncated=no` nếu toàn bộ đối tượng nằm trong ảnh, kể cả khi nó bị vật khác che. Chạm cạnh ảnh nhưng không bị cắt không tự động có nghĩa là `yes`.

### R-04 — Kích thước nhỏ và vùng bỏ qua

Ngưỡng chiều cao box lớp mục tiêu tối thiểu là 20 px. Nếu box dự kiến cao dưới 20 px, không tạo box lớp mục tiêu; dùng `ignore_region` quanh vùng mục tiêu nhỏ nhất có thể. Từ 20 px trở lên, gán lớp bình thường nếu nhận diện được. Không nới box để vượt ngưỡng.

Dùng `ignore_region` tương tự cho cụm mục tiêu quá dày không thể tách riêng. Không phủ lên các đối tượng lân cận vẫn gán được, và không dùng vùng này cho đối tượng ngoài phạm vi hoặc chỉ vì chưa rõ lớp.

### R-05 — Đối tượng ngoài phạm vi

Không gán người/phương tiện cho tượng, ma-nơ-canh, hình in hoặc ảnh phản chiếu; đồ chơi/mô hình thu nhỏ; xe đạp không động cơ; động vật, biển báo và các đối tượng không có lớp trong taxonomy. Không chuyển chúng sang lớp gần giống hoặc `ignore_region`. Nếu không chắc một vật có thuộc lớp mục tiêu hay không, bật `can_review` khi có thể xác định box hợp lý; nếu chưa thể xác định cả vùng/vật cần box, ghi vào danh sách vấn đề ảnh để reviewer xử lý.

### R-06 — Chọn lớp phương tiện

Phân loại theo cấu tạo nhìn thấy được: xe chở khách thông thường là `car`; xe có thùng/sàn hàng riêng, gồm pickup, là `truck`; xe thân hộp liền khối cỡ nhỏ/trung bình là `van`; xe buýt/xe khách quy mô lớn là `bus`; phương tiện cơ giới ba bánh là `tuk_tuk`; phương tiện cơ giới hai bánh là `motorcycle`. Khi hình ảnh không đủ để phân biệt, chọn lớp có căn cứ tốt nhất và bật `can_review`; không tạo lớp mới.

### R-07 — Xe được chở và hàng hóa

Gán box riêng cho xe chở và từng xe thuộc taxonomy đang được vận chuyển nếu xe đó nhìn thấy đủ để nhận diện và xác định box. Không đoán số lượng hoặc lớp xe bị che đến mức không nhận diện được. Hàng hóa không phải là một phương tiện thì không được gán thành phương tiện; box của xe chỉ bao phần xe nhìn thấy, không kéo rộng để bao hàng rời chất lên xe. Thiết bị cấu thành xe, gắn liền với thân xe được tính vào box xe.

### R-08 — Đối tượng mờ hoặc không chắc chắn

Đối tượng mờ/thiếu sáng vẫn được gán nếu còn nhận diện được lớp và ranh giới phần nhìn thấy; box ôm sát phần nhìn thấy và bật `can_review=true` nếu còn nghi ngờ đáng kể. `can_review` mặc định `false`, không thay thế cho việc chọn nhãn tốt nhất có thể và không dùng để bỏ qua một quy tắc đã rõ.

## 3. Kiểm soát chất lượng

Giữ nguyên các ngưỡng QC trong `submission/taxonomy.json`:

| Chỉ số                             | Ngưỡng |
| ---------------------------------- | -----: |
| Chiều cao box tối thiểu            |  20 px |
| IoU box lớn (chiều cao từ 50 px)   |   0.85 |
| IoU box nhỏ (chiều cao dưới 50 px) |   0.70 |
| IoU ghép cặp IAA                   |   0.50 |
| IAA: IoU trung bình                |   0.85 |
| IAA: cùng lớp                      |   0.95 |
| Tỷ lệ ảnh lỗi tối đa               |   0.02 |

Đánh giá IAA trên các cặp box ghép một-một; ghi mẫu số và xem xét riêng box không ghép, sai lớp, bỏ sót/thừa và vùng `ignore_region`. Không thay đổi ngưỡng khi áp dụng guideline này.

## 4. Mã quy tắc

Các quy tắc `R-01` đến `R-08` là mã tham chiếu ổn định để ghi chú ca khó, thảo luận calibration và ghi nhận lỗi QC. Khi cập nhật quy tắc có thể làm thay đổi nhãn, tăng phiên bản guideline và rà soát lại dữ liệu bị ảnh hưởng.

## Ghi nhận calibration v1.1

Phiên bản này giữ nguyên các quy tắc và taxonomy v1.0. Theo ủy quyền của người dùng, team chốt các ca IAA bằng đồng thuận đa số cho lớp và thuộc tính, trung bình tọa độ các box được ghép; vùng `ignore_region` chỉ giữ khi đạt đa số. Các box không có đa số được giữ theo consensus draft và được ghi nhận rõ trong bảng quyết định; kết quả không phải xác nhận độc lập từ người gán.
