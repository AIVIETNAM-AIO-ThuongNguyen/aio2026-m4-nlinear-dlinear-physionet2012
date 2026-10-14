# Neo dự báo và độ trễ quan sát trong dự báo chuỗi thời gian lâm sàng thưa: NLinear và DLinear trên PhysioNet 2012

Kho mã tái lập toàn bộ thực nghiệm của khảo sát thực nghiệm cho Module 4, AIO 2026 (Conquer, Slot 4). Mục tiêu của kho là để người đọc có thể: (1) đọc ngay kết quả đã chạy trong từng notebook mà không cần GPU, và (2) chạy lại từ đầu trên Google Colab để kiểm chứng từng con số.

## Câu hỏi nghiên cứu

- **H1.** Với dữ liệu ICU lấy mẫu bất thường, giá trị neo mà NLinear trừ đi và cộng lại có quyết định sai số không? Neo bằng phép đo thật gần nhất (neo B) có tốt hơn neo ở bước cuối (neo A) không, và lợi ích tập trung ở độ trễ quan sát nào?
- **H2.** Một nhánh tĩnh affine (tuổi, giới, chiều cao, loại ICU; thêm 16 tham số) có cải thiện neo B với hàm Huber không?

## Dữ liệu

PhysioNet/Computing in Cardiology Challenge 2012 (Silva et al., 2012; Goldberger et al., 2000), set-a và set-b, truy cập mở tại https://physionet.org/content/challenge-2012/1.0.0/ (doi:10.13026/C2301D). Dữ liệu **không** nằm trong kho này; notebook 02 tải về Google Drive của người chạy.

Giao thức chính: lưới giờ cố định, 36 giờ vào, 12 giờ ra, 37 kênh, chia theo hồ sơ bệnh nhân, chuẩn hóa chỉ trên tập huấn luyện, chấm sai số chỉ tại ô có phép đo thật. Tập kiểm tra gồm 1.585 hồ sơ và 136.752 điểm có phép đo thật.

## Cấu trúc Github repo này

```
.
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
├── Output/
│   └── processed/          # kết quả của lần chạy gốc (json, csv, png, pdf, txt)
└── notebooks/
    ├── DANH_MUC_NOTEBOOK.csv
    ├── 01-Chuan_bi_thu_muc_va_ma_nguon_doi_chieu.ipynb
    ├── ...
    └── 31-Hop_nhat_Holm_hinh_va_artefact.ipynb
```

## Cách chạy trên Google Colab

1. Mở notebook trên Colab (File, Open notebook, GitHub, dán đường dẫn kho). Chọn Runtime, Change runtime type, GPU T4.
2. Mọi notebook gắn Google Drive và dùng chung thư mục dự án `/content/drive/MyDrive/AIO2026_Conquer_M4_DLinear_NLinear_Slot4`. Đổi tham số `PROJECT_DIR` ở ô đầu nếu muốn đặt chỗ khác.
3. Chạy theo đúng thứ tự số 01 đến 31. Mỗi notebook ghi kết quả vào một thư mục con riêng trong `Output/processed/` và có chốt chặn kiểm tra đầu vào: nếu thiếu kết quả của notebook trước, notebook dừng với thông báo rõ cần chạy notebook nào.
4. Các notebook huấn luyện dùng lại lần chạy đã có trên Drive nếu trùng cấu hình. Đặt `CHAY_LAI_TU_DAU = True` (ở các notebook có tham số này) để huấn luyện lại hoàn toàn.
5. Notebook 27 lưu bản khai báo trước giả thuyết kèm mã băm SHA-256 trước khi chạy 28 đến 30; notebook 31 kiểm lại mã băm và thứ tự thời gian. Không sửa nội dung khai báo trong notebook 27 nếu muốn giữ tính hợp lệ của phép kiểm.

## Thứ tự chạy và phụ thuộc

#### A. Chuẩn bị và giao thức chính

| Số | Notebook | Thư mục kết quả | Cần chạy trước |
|---|---|---|---|
| 01 | `01-Chuan_bi_thu_muc_va_ma_nguon_doi_chieu.ipynb` | `không` | không |
| 02 | `02-Tai_du_lieu_PhysioNet_2012.ipynb` | `không` | 01 |
| 03 | `03-Tien_xu_ly_va_thuc_nghiem_chinh.ipynb` | `chot` | 02 |

#### B. Phương án neo và hàm mất mát

| Số | Notebook | Thư mục kết quả | Cần chạy trước |
|---|---|---|---|
| 04 | `04-Bon_phuong_an_neo.ipynb` | `bo_sung_04_neo_abcd` | 03 |
| 05 | `05-Ham_mat_mat_Huber.ipynb` | `bo_sung_06_huber` | 03 |
| 06 | `06-Bon_phuong_an_neo_nam_hat_giong.ipynb` | `bo_sung_13_neo_abcd_nam_hat_giong` | 03, 04 |
| 07 | `07-Huber_va_neo_B.ipynb` | `bo_sung_14_huber_neo_b` | 03, 04, 05 |
| 08 | `08-Co_che_neo.ipynb` | `bo_sung_11_co_che_neo` | 03, 04 |

#### C. Phân tích cơ chế và đối chứng

| Số | Notebook | Thư mục kết quả | Cần chạy trước |
|---|---|---|---|
| 09 | `09-Ba_ham_mat_mat_hai_neo.ipynb` | `w3_16_ba_ham_mat_mat_hai_neo` | 03, 04, 05, 07 |
| 10 | `10-Luoi_nam_hat_giong.ipynb` | `w3_17_luoi_nam_hat_giong` | 03, 04, 05, 06, 07 |
| 11 | `11-Chi_so_theo_nhom_kenh.ipynb` | `w3_18_chi_so_theo_nhom_kenh` | 03 |
| 12 | `12-Phan_tang_theo_do_tre.ipynb` | `w3_19_phan_tang_theo_do_tre` | 03, 11 |
| 13 | `13-Moc_trung_binh_dich_chuyen.ipynb` | `w3_20_moc_trung_binh_dich_chuyen` | 03, 04 |
| 14 | `14-Doi_chung_moc_manh_nhat.ipynb` | `w3_21_doi_chung_moc_manh_nhat` | 03, 04, 09, 11 |
| 15 | `15-Neo_B_ham_tuyet_doi_nam_hat_giong.ipynb` | `w3_22_neo_b_ham_tuyet_doi_nam_hat_giong` | 03, 04, 09, 11 |
| 16 | `16-Kiem_tra_thang_do_kenh.ipynb` | `w3_23_kiem_tra_thang_do_kenh` | 03, 11 |
| 17 | `17-Neo_theo_nguong_do_phu.ipynb` | `w3_24_neo_theo_nguong_do_phu` | 03, 11 |
| 18 | `18-Phan_tang_gop_tang_cuoi.ipynb` | `w3_25_phan_tang_gop_tang_cuoi` | 03, 11, 12 |
| 19 | `19-DLinear_neo_B_giao_thuc_chinh.ipynb` | `w3_31_dlinear_neo_b_giao_thuc_chinh` | 03 |
| 20 | `20-Neo_A_ham_tuyet_doi_nam_hat_giong.ipynb` | `w3_33_neo_a_ham_tuyet_doi_nam_hat_giong` | 03 |
| 21 | `21-Neo_theo_ty_le_trung_neo.ipynb` | `w3_32_neo_theo_ty_le_trung_neo` | 03 |

#### D. Hợp nhất và kiểm định

| Số | Notebook | Thư mục kết quả | Cần chạy trước |
|---|---|---|---|
| 22 | `22-Hop_nhat_chi_so.ipynb` | `w3_artefacts_tong_hop` | 03, 09, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21 |
| 23 | `23-Kiem_dinh_dong_nhat.ipynb` | `w3_artefacts_tong_hop` | 03, 09, 10, 11, 12, 13, 14, 15, 16, 17, 18, 22 |
| 24 | `24-Bang_kenh_va_phan_tang.ipynb` | `w3_artefacts_tong_hop` | 03, 09, 10, 11, 12, 13, 14, 15, 16, 17, 18, 22, 23 |
| 25 | `25-Bo_hinh_chinh.ipynb` | `w3_artefacts_tong_hop` | 03, 09, 10, 11, 12, 13, 14, 15, 16, 17, 18, 22, 23, 24 |
| 26 | `26-Goi_artefact_va_ban_trich.ipynb` | `w3_artefacts_tong_hop` | 03, 09, 10, 11, 12, 13, 14, 15, 16, 17, 18, 22, 23, 24, 25 |

#### E. Khai báo trước, H1, H2 và Holm

| Số | Notebook | Thư mục kết quả | Cần chạy trước |
|---|---|---|---|
| 27 | `27-Khai_bao_truoc_va_cong_kiem_tra.ipynb` | `w4_34_khai_bao_truoc_va_cong_tuan_4` | 03, 22 |
| 28 | `28-Quet_delta_Huber.ipynb` | `w4_35_quet_delta_huber` | 03, 27 |
| 29 | `29-Kiem_dinh_B_Huber_va_giam_K5.ipynb` | `w4_36_kiem_dinh_b_huber_va_giam_k5` | 03, 27, 28 |
| 30 | `30-Nhanh_tinh_H2.ipynb` | `w4_37_nhanh_tinh_h2` | 03, 27, 29 |
| 31 | `31-Hop_nhat_Holm_hinh_va_artefact.ipynb` | `w4_38_hop_nhat_holm_hinh_va_artefact_tuan_4` | 03, 27, 28, 29, 30 |

Notebook 22 có bước đối chiếu chéo tùy chọn với kết quả của notebook 29; khi chạy mới theo thứ tự, bước này tự bỏ qua và không ảnh hưởng kết quả.

Notebook 22 quét mọi thư mục `Output/processed/*/runs/` thuộc phạm vi tuần 2 và tuần 3 rồi gộp theo khóa cấu hình. Sổ đăng ký gốc gồm 101 lần huấn luyện, 76 lần được dùng; mọi lần được dùng đều do các notebook trong kho này sinh ra, nên chạy đủ 01 đến 21 là đủ đầu vào cho bước hợp nhất.

## Con số kiểm chứng nhanh

Các số dưới đây là MSE và MAE trung bình theo điểm trên thang đã chuẩn hóa, tập kiểm tra 1.585 hồ sơ, năm hạt giống. Chạy lại đúng giao thức phải cho số trùng đến sai khác do hạt giống GPU.

- Mô hình cơ sở dự báo duy trì (Persistence): MSE 0,578754; MAE 0,465943 (notebook 03).
- NLinear, neo B, hàm Huber (δ = 1): MSE 0,433972 ± 0,000291; MAE 0,431270 (notebook 10, 22). Giảm 25,0% MSE và 7,44% MAE so với Persistence.
- NLinear, neo A, hàm MSE: MSE 0,488224; MAE 0,471729.
- DLinear, neo B, hàm Huber: MSE 0,434091; MAE 0,431355 (notebook 19).
- Quét ngưỡng Huber (notebook 28): quy tắc khai báo trước chọn δ trên tập kiểm định và chọn δ = 2,0 (MSE kiểm định 0,477640 so với 0,478729 của δ = 1). Trên tập kiểm tra, δ = 2,0 so với δ = 1 chênh MSE +0,000748, p 0,0856, chưa phân biệt được; họ Holm dùng δ = 1 như đã khai báo.
- K01, neo B Huber trừ mốc giảm K5: hiệu MSE −0,042989, khoảng tin cậy 95% [−0,052008; −0,034116], p Holm 0,0012 (notebook 29, 31).
- H2, nhánh tĩnh: K03 p Holm 0,5264 và K05 p Holm 0,352, không đạt (notebook 30, 31).

## Kết quả đã chạy trong kho lưu trữ

Thư mục `Output/processed/` giữ kết quả của lần chạy gốc để người chấm đối chiếu mà không cần chạy lại. Mỗi thư mục con ứng với một notebook (xem cột `thu_muc_ket_qua` trong `notebooks/DANH_MUC_NOTEBOOK.csv`).

- `w3_artefacts_tong_hop/W3_HOP_NHAT_CHI_SO.json`: bảng hợp nhất 18 cấu hình và 13 phép kiểm định S01 đến S13.
- `w4_artefacts_tong_hop/W4_HOP_NHAT.json` và `W4_ban_trich_so_lieu.txt`: sáu phép thử K01 đến K06 với hiệu chỉnh Holm, mã băm bản khai báo trước.
- `*/runs/*.json`: chỉ số của từng lần huấn luyện; `*/figures/`: hình PNG và PDF.

Dữ liệu đã tiền xử lý (`*.npz`), trọng số mô hình (`*.pt`) và các gói nén không đưa lên kho; chạy lại notebook sẽ sinh lại đầy đủ.

## Mã định danh nội bộ

Trong tên khóa JSON, tên hình và nhật ký chạy còn xuất hiện các mã như `W3_18` hoặc `W4_36`. Đây là mã định danh cố định trong quá trình chạy gốc; chúng được giữ nguyên vì nằm trong nội dung đã băm của bản khai báo trước và trong các phép đối chiếu chéo giữa notebook. Bảng ánh xạ đầy đủ nằm trong `notebooks/DANH_MUC_NOTEBOOK.csv` và ở ô thứ hai của mỗi notebook.

Kết quả hiển thị trong notebook là kết quả của lần chạy gốc trên Colab (Tesla T4; Python 3.13.15; NumPy 2.1.3; PyTorch 2.11.0+cu130; SciPy 1.16.3).

## Phạm vi không bao gồm

Các phân tích phụ đã được thay thế bởi phiên bản năm hạt giống hoặc bởi bước hợp nhất chung (độ nhạy cách chia, giao thức theo sự kiện, phạm vi chuẩn hóa, các lần thử độ nhạy ban đầu) không nằm trong kho này vì có ý nghĩa trong kết quả cuối cùng.

## Báo cáo

Báo cáo cuối: https://drive.google.com/file/d/1wU3lexzK-5TT00WUYe9UcEfNkI6_BXnS/view?usp=sharing.

## Tài liệu tham khảo chính

- Goldberger, A.L. et al. (2000) ‘PhysioBank, PhysioToolkit, and PhysioNet’, *Circulation*, 101(23), pp. e215 đến e220.
- Holm, S. (1979) ‘A simple sequentially rejective multiple test procedure’, *Scandinavian Journal of Statistics*, 6(2), pp. 65 đến 70.
- Huber, P.J. (1964) ‘Robust estimation of a location parameter’, *Annals of Mathematical Statistics*, 35(1), pp. 73 đến 101.
- Silva, I. et al. (2012) ‘Predicting in-hospital mortality of ICU patients: the PhysioNet/Computing in Cardiology Challenge 2012’, *Computing in Cardiology*, 39, pp. 245 đến 248.
- Yalavarthi, V.K. et al. (2024) ‘GraFITi: graphs for forecasting irregularly sampled time series’, *AAAI*, 38(15), pp. 16255 đến 16263.
- Zeng, A. et al. (2023) ‘Are transformers effective for time series forecasting?’, *AAAI*, 37(9), pp. 11121 đến 11128.
