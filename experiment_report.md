# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-1803
**Name:** Nguyen Hung Nguyen
**Date:** 2026-06-10

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 10 | Trả về kết quả chính xác, sản phẩm thực tế và hợp lệ trong danh mục electronics. |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 1 | Trả về một sản phẩm cực đoan (outlier) với giá quá cao và phi thực tế. |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Khi sử dụng dữ liệu rác (Garbage Data), AI Agent đã trả về kết quả không chính xác ("Nuclear Reactor at $999999") do các nguyên nhân chính sau:
1. **Dữ liệu có giá trị ngoại lai cực hạn (Extreme Outliers):** Bản ghi "Nuclear Reactor" có giá trị $999999, vượt xa các giá trị bình thường khác. Do thuật toán chọn sản phẩm có giá trị lớn nhất (`idxmax` của cột price) trong danh mục `electronics` nên Agent bị đánh lừa bởi giá trị bất thường này.
2. **Trùng lặp ID (Duplicate IDs):** Bản ghi Laptop và Banana đều dùng chung ID 1, gây mất tính nhất quán trong cơ sở dữ liệu nếu truy vấn theo ID.
3. **Sai kiểu dữ liệu (Wrong Data Types):** Bản ghi "Broken Chair" có giá trị price là chuỗi "ten dollars" thay vì định dạng số thực/số nguyên. Điều này khiến cho các hàm tính toán toán học hoặc so sánh số lượng trên cột price bị lỗi hoặc không thể so sánh được nếu không được chuyển đổi và làm sạch đúng cách.
4. **Giá trị trống/thiếu (Null values):** Có các dòng dữ liệu bị thiếu hoàn toàn thông tin như không có ID, không có Category, hoặc có giá trị rỗng/None. Khi Agent thực hiện lọc theo tên danh mục (ví dụ category rỗng hoặc category không chuẩn hóa), nó sẽ bỏ sót các bản ghi hoặc trả về thông tin rỗng/lỗi hệ thống (Agent Error).

Do đó, nếu không có bước tiền xử lý (Data Cleaning/Validation), dữ liệu xấu sẽ làm sai lệch hoàn toàn kết quả phân tích hoặc suy luận của mô hình AI.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** Đồng ý.

Dù prompt có được thiết kế chi tiết và thông minh đến đâu, nếu dữ liệu đầu vào bị sai lệch, chứa thông tin rác, sai kiểu dữ liệu hay chứa các giá trị ngoại lai bất thường thì kết quả đầu ra của Agent vẫn sẽ sai lệch hoàn toàn (quy tắc "Garbage In, Garbage Out"). Một prompt tốt chỉ giúp Agent hiểu rõ yêu cầu công việc, còn dữ liệu chất lượng cao mới là cơ sở để Agent đưa ra quyết định chính xác và đáng tin cậy. Do đó, việc xây dựng một data pipeline sạch và có cơ chế observability (quan sát/kiểm định) là vô cùng quan trọng.
