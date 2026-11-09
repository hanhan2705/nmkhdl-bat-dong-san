# Đồ án Nhập môn Khoa học dữ liệu

**Đề tài:** Xây dựng bộ dữ liệu về bất động sản, thực hiện các phương pháp tối ưu hiệu năng cho hệ thống khi dữ liệu quá lớn.

**Thành viên**
- Bùi Kiếm Khoa – n23dccn029
- Lương Võ Hân Hân – n23dccn017

## 1. Giới thiệu

Đồ án tập trung vào quy trình thu thập, làm sạch, phân tích dữ liệu bất động sản và so sánh hiệu năng của một số cách đọc, lưu trữ và truy vấn dữ liệu.

Nhóm sử dụng hai bộ dữ liệu riêng biệt:
- **UCI Real Estate Valuation:** dữ liệu bất động sản Đài Loan gồm 414 dòng. Từ dữ liệu này, chương trình tạo các tập dữ liệu mô phỏng có kích thước lớn hơn để phục vụ benchmark. Các tập mở rộng là dữ liệu tổng hợp, không phải dữ liệu giao dịch mới.
- **Vietnam Real Estate:** dữ liệu tin đăng bất động sản Việt Nam do TiniX AI công bố. Repo kèm bản trích 9 cột cấu trúc từ 4 shard đầu, tổng cộng 1.400.300 dòng nguồn; không phải toàn bộ 3,5 triệu dòng được công bố.

Hai bộ dữ liệu được xử lý và đánh giá riêng, không gộp các mức giá có đơn vị khác nhau.

## 2. Nội dung thực hiện

1. Thu thập và tìm hiểu nguồn dữ liệu.
2. Kiểm tra dữ liệu thiếu, dữ liệu không hợp lệ và bản ghi trùng.
3. Làm sạch, chuẩn hóa và lưu dữ liệu ở định dạng CSV, Parquet.
4. Khám phá dữ liệu bằng thống kê và biểu đồ.
5. Thực nghiệm các phương pháp đọc và truy vấn dữ liệu.
6. So sánh thời gian thực thi, mức sử dụng bộ nhớ và tính nhất quán của kết quả.

Các phương pháp benchmark chính gồm:
- Đọc toàn bộ CSV bằng Pandas.
- Đọc CSV với các cột cần thiết.
- Đọc CSV có khai báo kiểu dữ liệu.
- Đọc CSV theo từng khối (chunk).
- Đọc Parquet với các cột cần thiết.
- Truy vấn Parquet bằng DuckDB.

## 3. Công nghệ sử dụng

- Python
- Pandas, NumPy
- PyArrow, Parquet
- DuckDB
- Matplotlib
- psutil
- Jupyter Notebook

## 4. Cấu trúc repository

```text
.
├── data/
│   ├── raw/                 # Dữ liệu nguồn UCI
│   ├── processed/           # Dữ liệu UCI sau tiền xử lý
│   └── vietnam/             # Dữ liệu bất động sản Việt Nam
├── notebooks/               # Notebook thực hiện đồ án
├── src/                     # Các module xử lý và thực nghiệm
├── results/                 # Số liệu, cấu hình và biểu đồ
├── report/                  # Báo cáo, slide
├── tests/                   # Các kiểm thử
├── demo.py                  # Chạy thử một truy vấn
├── run_project.py           # Chạy quy trình với dữ liệu UCI
├── run_vietnam.py           # Chạy quy trình với dữ liệu Việt Nam
├── run_validation.py        # Chạy các kiểm chứng bổ sung
├── search_vietnam.py        # Tra cứu tin đăng Việt Nam
└── requirements.txt
```

## 5. Cài đặt

Yêu cầu Python tương thích với các phiên bản thư viện trong `requirements.txt`.

Trên Windows, mở PowerShell tại thư mục dự án và chạy:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Nếu PowerShell không cho phép kích hoạt môi trường ảo, có thể gọi Python trực tiếp bằng đường dẫn `.venv\Scripts\python.exe`.

## 6. Chạy chương trình

### Dữ liệu UCI

Chạy chuẩn bị dữ liệu, tạo các tập benchmark và thực nghiệm:

```powershell
python run_project.py
```

Mặc định chương trình tạo các tập 100.000, 500.000 và 1.000.000 dòng, sau đó chạy benchmark. Có thể thay đổi kích thước và số lần lặp:

```powershell
python run_project.py --sizes 100000 500000 1000000 --repeats 3
```

### Dữ liệu Việt Nam

Dữ liệu nguồn và dữ liệu đã xử lý được lưu trong repo. Chạy chuẩn bị và benchmark:

```powershell
python run_vietnam.py
```

Chạy thêm các thực nghiệm mở rộng:

```powershell
python run_vietnam.py --extended
```

### Kiểm chứng bổ sung

Sau khi đã chuẩn bị dữ liệu benchmark Việt Nam:

```powershell
python run_validation.py
```

### Demo truy vấn

Ví dụ demo trên dữ liệu UCI:

```powershell
python demo.py --dataset uci --method duckdb_parquet --rows 1000000
```

Ví dụ demo trên dữ liệu Việt Nam:

```powershell
python demo.py --dataset vietnam --method duckdb_parquet --rows 1000000
```

Tra cứu tin đăng tại Hà Nội và xuất kết quả ra CSV:

```powershell
python search_vietnam.py --province "Hà Nội" --property-type "Nhà" --area-min 50 --area-max 100 --price-max 5000000000 --limit 10 --export results/ha_noi.csv
```

## 7. Báo cáo và kết quả

Có thể mở trực tiếp các file HTML trong thư mục `report/` bằng trình duyệt:

| Tệp | Nội dung |
|---|---|
| `bao_cao.html` | Báo cáo chính của đồ án |
| `thuyet_trinh.html` | Slide thuyết trình |
| `bo_sung_du_lieu_viet_nam.html` | Phần dữ liệu và thực nghiệm Việt Nam |
| `nang_cap_he_thong.html` | Các nội dung nâng cấp quy trình xử lý |
| `kiem_chung_du_lieu_lon.html` | Kiểm chứng workload, giới hạn bộ nhớ và hòa vốn chuyển đổi |

Một số kết quả tổng hợp:
- `results/summary.csv`: kết quả benchmark UCI.
- `results/measurements.csv`: các lần đo UCI.
- `results/environment.json`: thông tin môi trường thực nghiệm.
- `results/vietnam/summary.csv`: kết quả benchmark dữ liệu Việt Nam.
- `results/vietnam/`: các kết quả thực nghiệm bổ sung.

## 8. Nguồn dữ liệu và lưu ý

- UCI Real Estate Valuation: [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/477/real+estate+valuation+data+set), DOI: `10.24432/C5J30W`, giấy phép CC BY 4.0.
- Vietnam Real Estate: [TiniX AI trên Hugging Face](https://huggingface.co/datasets/tinixai/vietnam-real-estates), giấy phép CC BY-NC 4.0. Dữ liệu được sử dụng cho mục đích học tập phi thương mại và cần ghi công nguồn.

Dữ liệu Việt Nam là **tin đăng**, không phải dữ liệu giao dịch đã xác minh. Nguồn có cả tin bán và cho thuê nhưng tập cột được sử dụng không có nhãn phân biệt; vì vậy không diễn giải kết quả trung bình thành giá giao dịch hoặc giá bán của thị trường.

Các tập dữ liệu mô phỏng từ UCI chỉ dùng để tạo tải cho thực nghiệm hiệu năng. Không sử dụng chúng để đưa ra kết luận về thị trường bất động sản.
