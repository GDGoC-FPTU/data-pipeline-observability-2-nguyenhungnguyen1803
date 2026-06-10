[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112926&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** hungnguyen1803@example.com
**Name:** Nguyen Hung Nguyen

---

## Mo ta

Bài Lab này thực hiện thiết lập một quy trình ETL tự động hóa cơ bản bằng Python và thư viện Pandas. Quy trình này bao gồm:
1. **Extract:** Đọc dữ liệu thô từ file định dạng JSON (`raw_data.json`).
2. **Validate:** Kiểm soát chất lượng dữ liệu đầu vào (loại bỏ các sản phẩm có giá không hợp lệ (<= 0) hoặc danh mục sản phẩm bị bỏ trống).
3. **Transform:** Thực hiện chuẩn hóa danh mục (chuyển sang kiểu Title Case), áp dụng mức giảm giá 10% để tính cột giá mới `discounted_price`, đồng thời ghi nhận dấu thời gian xử lý dữ liệu `processed_at`.
4. **Load:** Lưu trữ tập dữ liệu sạch kết quả ra file `processed_data.csv`.

Bên cạnh đó, bài lab tiến hành một cuộc thử nghiệm (Stress Test) mô phỏng AI Agent làm việc với hai tệp dữ liệu sạch và rác để đánh giá ảnh hưởng của chất lượng dữ liệu đối với hiệu suất của mô hình AI.

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas pytest
```

### Chay ETL Pipeline
```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)
```bash
# Đầu tiên cần tạo dữ liệu rác
python generate_garbage.py

# Sau đó chạy kiểm thử mô phỏng Agent trên dữ liệu sạch và dữ liệu rác
python agent_simulation.py
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── processed_data.csv       # Output cua pipeline
├── experiment_report.md     # Bao cao thi nghiem
└── README.md                # File nay
```

---

## Ket qua

- Tổng số bản ghi thô (raw records) được trích xuất: 5 bản ghi.
- Số lượng bản ghi hợp lệ sau khi validate: 3 bản ghi (Laptop, Chair, Monitor).
- Số lượng bản ghi bị loại bỏ: 2 bản ghi (Mystery Box có giá âm -10, và Phone có danh mục rỗng).
- Kết quả mô phỏng Agent:
  - Khi dùng dữ liệu sạch: Agent đưa ra lựa chọn chính xác là Laptop với giá $1200.
  - Khi dùng dữ liệu rác: Agent bị đánh lừa bởi giá trị ngoại lai và lựa chọn Nuclear Reactor với giá $999999.
