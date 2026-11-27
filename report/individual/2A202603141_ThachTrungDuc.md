# Báo cáo Day 14 — Thạch Trung Đức — 2A202603141

- Mã học viên dùng với `--student`: `141`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: `score_thr` (tăng ngưỡng tin cậy từ mức gốc `0.1` lên khoảng `0.3`–`0.4`).
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): Quan sát ảnh BEV + 7 camera trong `d14_141/png_A/` và số đo cấu hình gốc ở bước 5 (`dự đoán 90`, `GT 25`, `xóa 66`, `thêm 1`, `sửa 2`, chi phí `21.8`), phần lớn công sửa đến từ **box thừa** (`66 × 0.3 = 19.8` điểm, chiếm hơn 90% tổng chi phí), chủ yếu là các box dự đoán nhầm vào bụi cây, dải phân cách, hàng rào và lề đường ở cả 3 frame (`s01f33` xóa 25, `s03f31` xóa 26, `s04f12` xóa 15), trong khi chỉ sót 1 box (xe `Bus` ở `s03f31` do model KITTI không học lớp Bus). Vì các box nhiễu ở hàng cây/lề đường thường có `score` thấp hơn xe thật, tăng `score_thr` sẽ loại được nhiều box thừa nhất; tuy nhiên không nên tăng quá cao (như `0.5`) vì mất 1 vật thể thật bị phạt `1.0` điểm, nặng hơn gấp hơn 3 lần xóa 1 box thừa (`0.3`). Còn `min_points` chỉ lọc thêm các box quá thưa điểm, khó lọc hết bụi cây dày điểm.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | | | | |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_141/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_141/frames_B.txt --config d14_141/best_config.json --gt d14_141/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
