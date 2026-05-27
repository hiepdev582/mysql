# MySQL

## MySQL basic

### I. Sorting data by custom list

```sql
ORDER BY FIELD(status, 'In Process', 'On Hold', 'Cancelled', 'Resolved');
```

### II. Using clause in JOIN

```sql
SELECT * FROM orders JOIN customers USING (customer_id);
```

### III. ROLLUP

- `ROLLUP`: là một phần mở rộng của mệnh đề `GROUP BY`. Thay vì chỉ tạo ra một nhóm duy nhất, nó tạo ra nhiều nhóm theo phân cấp để tính toán các dòng tổng phụ và dòng tổng cộng cuối cùng trong cùng một truy vấn.
- `GROUPING(column_name)`: Trả về 1 nếu dòng đó là dòng tổng hợp (super-aggregate), và 0 nếu là dòng dữ liệu thông thường.

```sql
SELECT
    IF(GROUPING(orderYear), 'All Years', orderYear) AS orderYear,
    IF(GROUPING(productLine), 'All Product Lines', productLine) AS productLine,
    SUM(orderValue) AS totalOrderValue
FROM sales
GROUP BY orderYear, productLine
WITH ROLLUP;
```

### IV. Temporary Table

```sql
CREATE TEMPORARY TABLE temp_table_name (
    column1_name data_type,
    column2_name data_type,
    ...
);
```

- Dùng khi:
  - Xử lý dữ liệu phức tạp: Khi bạn cần thực hiện các truy vấn đa bước, việc lưu kết quả trung gian vào bảng tạm giúp câu lệnh chính trở nên đơn giản và dễ tối ưu hơn.
  - Dữ liệu tạm: Khi bạn cần lưu trữ các tập dữ liệu không cần thiết phải duy trì lâu dài trong hệ thống.
  - Tối ưu hóa: Sử dụng bảng tạm làm "bộ đệm" để thực hiện tính toán trên dữ liệu lớn trước khi đưa vào bảng chính thức.
