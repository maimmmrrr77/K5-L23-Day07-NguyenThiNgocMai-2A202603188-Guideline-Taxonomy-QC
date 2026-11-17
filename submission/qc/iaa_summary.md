# Tóm tắt IAA (taxonomy v1.0)

- Ảnh được so sánh (có >=2 người gán): **40**; số người gán: 3
- Lượt gán đủ **80/80**.
- IoU trung bình và % cùng lớp **chỉ tính trên các cặp box đã ghép** (IoU >= 0.5, ghép 1-1 không phân biệt lớp). Box chưa ghép được thống kê riêng ở cột bỏ sót/thừa.
- Mẫu số: 187 cặp box ghép (tổng mọi cặp người gán, ảnh chung).

| Cặp | Ảnh | Cặp box ghép | IoU TB | Cùng lớp | Cùng occluded_level | Cùng truncated | Chưa ghép (A/B) | Bỏ qua do ignore | Đạt IAA |
|---|---|---|---|---|---|---|---|---|---|
| **TỔNG** | 40 | 187 | 0.880 | 95.7% | 52.4% | 90.4% | 190/25 | 0 | ĐẠT |
| 188|193 | 13 | 93 | 0.854 | 93.5% | 54.8% | 88.2% | 28/12 | 0 | CHƯA ĐẠT |
| 188|241 | 13 | 64 | 0.948 | 96.9% | 34.4% | 92.2% | 87/4 | 0 | ĐẠT |
| 193|241 | 14 | 30 | 0.814 | 100.0% | 83.3% | 93.3% | 75/9 | 0 | CHƯA ĐẠT |

Ngưỡng IAA: IoU TB >= 0.85, đồng thuận lớp >= 95%.

Số ca bất thường: 393 ({'bo_sot_thua': 213, 'sai_thuoc_tinh': 95, 'iou_thap': 62, 'sai_lop': 8, 'bat_dong_ignore': 9, 'co_can_review': 6}), trên 39 ảnh cần calibration.
