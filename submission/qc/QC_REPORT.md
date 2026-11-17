# Báo cáo QC — Day 07 (tên nhóm không ghi trong hồ sơ)

## 1. Mục đích, dữ liệu và phiên bản

- Mục đích và giới hạn sử dụng: Thực hành đặc tả và QC nhãn bounding box 2D người/phương tiện; bộ ảnh tuyển chọn cho ca biên, chưa đánh giá tính đại diện cho dữ liệu vận hành.
- Nguồn và phép sử dụng dữ liệu: 40 ảnh; nguồn, tác giả, giấy phép và URL từng ảnh được ghi trong `data/ATTRIBUTION.csv` (CC BY, CC BY-SA, CC0 và Public Domain).
- Manifest và checksum nhãn cuối: `submission/qc/batch_manifest.json`; SHA-256 manifest `adf96cfbef09c58eff9b1f424bff7860269c1a5ca7476f6ffaf308e9f438aa3b`; SHA-256 nhãn `submission/qc/labels_calibrated.json`: `c8d3fe1288af9cd0478907d84357a4eecae83cfa6b6a99db5e00e9a1e2258f3e`. Snapshot vòng 1 được giữ nguyên.
- Guideline và taxonomy khi gán / khi bàn giao: IAA trước calibration theo taxonomy v1.0; nhãn bàn giao theo taxonomy v1.0 và guideline v1.1 calibrated. v1.1 ghi nhận cách calibration, không đổi taxonomy hoặc các quy tắc R-01–R-08.

## 2. Phân công và đồng thuận trước calibration

- Lượt hoàn thành / được giao: 80/80 lượt; 3 người gán (mã 188, 193, 241), theo `submission/qc/iaa_summary.md`.
- Số cặp box ghép, IoU TB, đồng thuận lớp và mẫu số: 187 cặp ghép; IoU TB 0.880; cùng lớp 179/187 = 95.7%. Chỉ số trên các cặp ghép 1–1, IoU ghép ≥ 0.5.
- Box chưa ghép, loại do ignore, bất đồng vùng ignore: 190/25 box chưa ghép (A/B, tổng các cặp người gán); 0 cặp ghép bị loại do ignore; 9 ca bất đồng ignore được ghi trong thống kê bất thường IAA.
- Ca hai người có thể cùng sai sau khi đối chiếu tham chiếu: IAA đo đồng thuận, không chứng minh nhãn đúng. Các điểm cần đối chiếu ảnh gốc/overlay gồm box gộp ở `img_030.jpg` (R-01, R-04; object 282, 286, 287), box rộng ở `img_014.jpg` (R-02, R-08; object 139, 140), và vùng xe/người ở `img_040.jpg` (R-02; object 400, 401); reviewer cần xác nhận ranh giới theo ảnh gốc.

## 3. Calibration và kiểm lại

- Ca đã xử lý, rule ID, bản sửa và người xác nhận: giữ 393 quyết định calibration ban đầu và thêm 13 ca Rework `Q01`–`Q13` trong `submission/qc/calibration_decisions.csv` (406 dòng tổng). Đã sửa box theo R-01, R-02, R-04, R-06, R-07, R-08; reviewer 241 được chỉ định xác nhận vòng 2.
- Tác động v1.1 lên nhãn và ảnh: Pha 5 tạo lại nhãn cho 40 ảnh; 403 annotation (trước sửa 402), gồm 9 box đã siết/chia, 4 cờ `can_review` đã clear và 1 box xe máy mới tại `img_030.jpg`. Không đổi taxonomy hoặc mapping lớp. Hai box motion blur (`img_014.jpg` #139, #140) vẫn `can_review=true`, cần reviewer xác nhận.
- Tham chiếu bằng chứng khi Rework: vòng 1 và phiếu `submission/qc/reviews/round-01/qc_review.csv`; sửa theo log `Q01`–`Q13`, xuất Pha 5 `labels_calibrated_5bc172e9ac9ba9ff.json`, rồi dựng lại `labels_calibrated.json`. Vòng 2 full đã tạo; kết quả kiểm lại đang chờ reviewer.

## 4. Kết quả duyệt

- Vòng QC: 2
- Người review và ngày: 241 pre-review được ghi theo yêu cầu dưới mã 241 ngày 2026-11-17; reviewer 241 cần xác nhận độc lập trước khi coi là duyệt chính thức.
- Đã duyệt / cả lô: 40/40 ảnh có kết quả 241 pre-review; phiếu là rà sơ bộ, chưa có reviewer 241 sign-off.
- Ảnh lỗi / ảnh đã duyệt: 241 pre-review ghi nhận 0/40 = 0.0%; đây chưa phải kết quả nghiệm thu độc lập. Vòng 1 ghi nhận 8/40 = 20.0%, vượt ngưỡng lab ≤ 2%.
- Annotation đối tượng hiện có bị lỗi / annotation đối tượng đã duyệt: 241 pre-review ghi nhận 0/403 = 0.0%; cần reviewer 241 xác nhận. Vòng 1 ghi nhận 13/402 = 3.2%.
- Vật bỏ sót, ảnh và vùng: 241 pre-review không ghi nhận vật bỏ sót (0/40 ảnh); vòng 1 cũng không ghi nhận ca bỏ sót (0 ca). Reviewer cần xác nhận ảnh gốc.
- Ca khó / phần còn lại, lỗi lặp và ví dụ: `Q01`–`Q13` ghi các sửa ở `img_014.jpg`, `img_015.jpg`, `img_016.jpg`, `img_019.jpg`, `img_028.jpg`, `img_030.jpg`, `img_034.jpg`, `img_040.jpg`. Reviewer 241 cần xác nhận box chia mới ở `img_030.jpg`, h241 box motion blur còn `can_review=true` ở `img_014.jpg`, và mọi kết quả `khong` trong phiếu 241.
- Một lỗi máy chặn được và một lỗi cần người đọc ảnh: Máy chặn giữ Hold vì nhãn còn 2 `can_review=true` và 241 pre-review chưa được reviewer 241 xác nhận; người cần đối chiếu overlay/ảnh gốc để xác nhận box sửa, phân tách xe ở `img_030.jpg` và chốt hai ca motion blur.

## 5. Quyết định bàn giao

- Quyết định: Hold
- Lý do, người nhận và phạm vi sử dụng: Theo cây Hold → Rework → Release, vòng 1 kích hoạt Rework; đã ghi `Q01`–`Q13`, sửa Pha 5 và tạo vòng 2 full. Phiếu 40 ảnh hiện là AI pre-review theo yêu cầu, chưa phải xác nhận độc lập của reviewer 241; nhãn còn 2 `can_review=true`. Chỉ dùng trong bài thực hành Day 07, không dùng như dữ liệu vận hành.
- Nếu còn việc: Reviewer 241 cần đối chiếu cả 40 ảnh/overlay, xác nhận hoặc sửa từng dòng trong `submission/qc/reviews/round-02/qc_review.csv`, xác nhận box mới/chỉnh, xử lý hai cờ `can_review` nếu đủ bằng chứng, rồi chạy lại `python scripts/qc_report.py --round 2`. Chỉ xem xét Release khi review toàn lô độc lập đạt ngưỡng ≤ 2% (với 40 ảnh cần 0 ảnh lỗi), không còn cờ chưa chốt và reviewer phê duyệt.
- Kiểm ngược từ báo cáo tới ảnh, rule và nhãn: `round-01/qc_review.csv` → `Q01`–`Q13` trong `calibration_decisions.csv` → Pha 5 export → `labels_calibrated.json` SHA-256 `c8d3fe1288af9cd0478907d84357a4eecae83cfa6b6a99db5e00e9a1e2258f3e` → `round-02/qc_review.csv` (40 dòng 241 pre-review, reviewer 241 cần xác nhận). Ví dụ `img_030.jpg` #282, #286, #287/R-01,R-04 và box mới #291; `img_014.jpg` #139–140/R-02,R-08.
