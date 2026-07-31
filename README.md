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

### V. Generated Columns

1. Loại:
   - **Stored Generated Columns**: Giá trị được tính toán khi hàng được chèn hoặc cập nhật và lưu trữ vật lý trên đĩa. Nó chiếm dung lượng nhưng đọc dữ liệu rất nhanh.
   - **Virtual Generated Columns**: Giá trị không được lưu trữ mà chỉ được tính toán "tức thời" mỗi khi bạn truy vấn. Nó tiết kiệm dung lượng đĩa nhưng tốn tài nguyên CPU khi đọc. (Đây là lựa chọn mặc định nếu bạn không chỉ định).

2. Cú pháp:

```sql
ALTER TABLE table_name
ADD column_name data_type GENERATED ALWAYS AS (expression) [VIRTUAL | STORED];
```

### VI. CTE

1. Cú pháp:

```sql
WITH cte_name AS (
    SELECT column1, column2,
    FROM table_name
    WHERE condition
)
SELECT * FROM cte_name;
```

2. Recursive CTE

```sql
WITH RECURSIVE category_path AS (
    -- Lấy danh mục gốc (parent_id là NULL)
    SELECT id, name, parent_id
    FROM categories WHERE parent_id IS NULL

    UNION ALL

    -- Lấy các danh mục con bằng cách nối (join) với bảng categories
    SELECT c.id, c.name, c.parent_id
    FROM categories c
    INNER JOIN category_path cp ON cp.id = c.parent_id
)
SELECT * FROM category_path;
```

### VII. Stored Procedure

```sql
DELIMITER //

CREATE PROCEDURE TenThuTuc()
BEGIN
    -- Các câu lệnh SQL nằm ở đây
    SELECT * FROM table_name;
END //

DELIMITER ;
```
