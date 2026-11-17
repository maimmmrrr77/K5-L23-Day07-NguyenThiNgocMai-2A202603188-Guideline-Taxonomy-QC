# Thông tin nhóm

- Tên nhóm: Nhom01
- Mã thành viên (ba chữ số): 193-Cao Thị Thúy Hiền, 188- Nguyễn Thị Ngọc Mai, 241- Nguyễn Mạnh Quang
- Người điều phối: Nguyễn Thị Ngọc Mai
- Người review bản cuối: Cao Thị Thúy Hiền ,Nguyễn Mạnh Quang
- Commit chốt bài (điền ở kênh nộp sau khi commit): https://github.com/maimmmrrr77/K5-L23-Day07-Nhom01-Guideline-Taxonomy-QC

## Kết quả tóm tắt (điền cuối bài)

- Lượt gán hoàn tất / được giao: **80/80**; mỗi ảnh có 2 lượt gán. Phân công: 188 **26/26** ảnh, 193 **27/27**, 241 **27/27**.
- IAA trước calibration (kèm mẫu số): **187** cặp box ghép; IoU trung bình **0.880**; cùng lớp **179/187 = 95.7%**. IAA được tính trên cặp ghép 1–1 với IoU từ 0.5; đồng thuận thuộc tính `occluded_level` là **52.4%**, cần lưu ý.
- Số ảnh phải calibration: **39/40** ảnh có candidate; **393** dòng candidate. Sau đó ghi thêm **13** ca Rework `Q01`–`Q13`.
- Vòng QC và số ảnh đã duyệt / cả lô: Vòng 1 **40/40**; vòng 2 **40/40 AI pre-review**, chưa có xác nhận độc lập của reviewer 241.
- Tỉ lệ lỗi: Vòng 1 **20.0% theo ảnh (8/40)** và **3.2% theo annotation (13/402)**. Vòng 2 AI pre-review ghi nhận **0.0% (0/40 ảnh)** và **0.0% (0/403 annotation)**; số liệu vòng 2 là sơ bộ, chưa phải nghiệm thu của người review.
- **Quyết định** (Release / Hold / Rework) và lý do một dòng: **Hold** — vòng 1 vượt ngưỡng nên đã Rework; vòng 2 chưa được reviewer 241 xác nhận độc lập và còn **2** annotation `can_review=true`.
