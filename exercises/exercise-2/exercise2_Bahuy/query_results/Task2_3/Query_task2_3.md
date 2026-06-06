1. SELECT 
    department,
    COUNT(*) as employee_count,
    AVG(salary) as avg_salary,
    MAX(salary) as max_salary,
    MIN(salary) as min_salary,
    STDDEV(salary) as salary_stddev,
    RANK() OVER (ORDER BY AVG(salary) DESC) as salary_rank
FROM employees_managed
GROUP BY department
ORDER BY avg_salary DESC;

+--------------+-----------------+-------------+-------------+-------------+---------------------+--------------+
|  department  | employee_count  | avg_salary  | max_salary  | min_salary  |    salary_stddev    | salary_rank  |
+--------------+-----------------+-------------+-------------+-------------+---------------------+--------------+
| Engineering  | 4               | 78750.0     | 82000.0     | 75000.0     | 2586.02010819715    | 1            |
| Sales        | 2               | 71000.0     | 72000.0     | 70000.0     | 1000.0              | 2            |
| Marketing    | 2               | 66000.0     | 67000.0     | 65000.0     | 1000.0              | 3            |
| HR           | 2               | 61000.0     | 62000.0     | 60000.0     | 1000.0              | 4            |
| 2            | 2               | 850.0       | 1200.0      | 500.0       | 350.0               | 5            |
| 1            | 4               | 612.5       | 1200.0      | 300.0       | 357.72720053135464  | 6            |
| 3            | 2               | 437.5       | 800.0       | 75.0        | 362.5               | 7            |
| 4            | 1               | 150.0       | 150.0       | 150.0       | 0.0                 | 8            |
| 5            | 1               | 25.0        | 25.0        | 25.0        | 0.0                 | 9            |
| NULL         | 71              | NULL        | NULL        | NULL        | NULL                | 10           |
+--------------+-----------------+-------------+-------------+-------------+---------------------+--------------+

=> Insights: Lần lượt in ra các department kèm với số nhân viên, trung bình lương, max lương, min lương, độ lệch lương và xếp hạng trung bình lương của department tương ứng. Engineer department có slg nhân viên cao nhát nhưng đồng thời sự chênh lệch về lương của các nhân viên cũng lớn nhất trong các department khác 

2.
SELECT 
    e.employee_id,
    e.first_name,
    e.last_name,
    e.department,
    COUNT(s.sale_id) as total_sales,
    SUM(s.revenue) as total_revenue,
    AVG(s.revenue) as avg_sale_value,
    MAX(s.sale_date) as last_sale_date
FROM employees_managed e
LEFT JOIN sales_fact s ON e.employee_id = s.employee_id
GROUP BY e.employee_id, e.first_name, e.last_name, e.department
ORDER BY total_revenue DESC;

+----------------+---------------+--------------+---------------+--------------+----------------+-----------------+-----------------+
| e.employee_id  | e.first_name  | e.last_name  | e.department  | total_sales  | total_revenue  | avg_sale_value  | last_sale_date  |
+----------------+---------------+--------------+---------------+--------------+----------------+-----------------+-----------------+
| 1004           | Alice         | Williams     | Sales         | 5            | 6825.0         | 1365.0          | 2024-01-23      |
| 1008           | Frank         | Wilson       | Sales         | 5            | 2375.0         | 475.0           | 2024-01-24      |
| 2010           | 1008          | 5010         | 1             | 0            | NULL           | NULL            | NULL            |
| 2009           | 1004          | 5009         | 1             | 0            | NULL           | NULL            | NULL            |
| 2008           | 1008          | 5008         | 4             | 0            | NULL           | NULL            | NULL            |
| 2007           | 1004          | 5007         | 3             | 0            | NULL           | NULL            | NULL            |
| 2006           | 1008          | 5006         | 2             | 0            | NULL           | NULL            | NULL            |
| 2005           | 1004          | 5005         | 1             | 0            | NULL           | NULL            | NULL            |
| 2004           | 1008          | 5004         | 1             | 0            | NULL           | NULL            | NULL            |
| 2003           | 1004          | 5003         | 3             | 0            | NULL           | NULL            | NULL            |
| 2002           | 1008          | 5002         | 5             | 0            | NULL           | NULL            | NULL            |
| 2001           | 1004          | 5001         | 2             | 0            | NULL           | NULL            | NULL            |
| 1010           | Henry         | Taylor       | HR            | 0            | NULL           | NULL            | NULL            |
| 1009           | Grace         | Moore        | Engineering   | 0            | NULL           | NULL            | NULL            |
| 1007           | Eve           | Miller       | Marketing     | 0            | NULL           | NULL            | NULL            |
| 1006           | Diana         | Davis        | Engineering   | 0            | NULL           | NULL            | NULL            |
| 1005           | Charlie       | Brown        | HR            | 0            | NULL           | NULL            | NULL            |
| 1003           | Bob           | Johnson      | Engineering   | 0            | NULL           | NULL            | NULL            |
| 1002           | Jane          | Doe          | Marketing     | 0            | NULL           | NULL            | NULL            |
| 1001           | John          | Smith        | Engineering   | 0            | NULL           | NULL            | NULL            |
| NULL           |               | NULL         | NULL          | 0            | NULL           | NULL            | NULL            |
| NULL           | NULL          | NULL         | NULL          | 0            | NULL           | NULL            | NULL            |
+----------------+---------------+--------------+---------------+--------------+----------------+-----------------+-----------------+

=> Insights: gồm id, họ tên và phòng ban của các nhân viên. Đồng thời tổng đơn đã hoàn thành, kèm với doanh thu và đơn sale hoàn thành gần nhất.


3.

SELECT 
    sale_year,
    sale_month,
    region,
    COUNT(*) as num_sales,
    SUM(revenue) as total_revenue,
    AVG(revenue) as avg_revenue,
    LAG(SUM(revenue)) OVER (PARTITION BY region ORDER BY sale_year, sale_month) as prev_month_revenue,
    ((SUM(revenue) - LAG(SUM(revenue)) OVER (PARTITION BY region ORDER BY sale_year, sale_month)) / 
    LAG(SUM(revenue)) OVER (PARTITION BY region ORDER BY sale_year, sale_month)) * 100 as revenue_growth_pct
FROM sales_fact
GROUP BY sale_year, sale_month, region
ORDER BY region, sale_year, sale_month;

+------------+-------------+---------+------------+----------------+--------------+---------------------+---------------------+
| sale_year  | sale_month  | region  | num_sales  | total_revenue  | avg_revenue  | prev_month_revenue  | revenue_growth_pct  |
+------------+-------------+---------+------------+----------------+--------------+---------------------+---------------------+
| NULL       | NULL        | NULL    | 71         | NULL           | NULL         | NULL                | NULL                |
| 2019       | 9           | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2019       | 11          | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2020       | 1           | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2020       | 2           | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2020       | 5           | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2020       | 7           | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2020       | 10          | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2020       | 12          | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2021       | 1           | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2021       | 3           | NULL    | 1          | NULL           | NULL         | NULL                | NULL                |
| 2024       | 1           | North   | 5          | 6825.0         | 1365.0       | NULL                | NULL                |
| 2024       | 1           | South   | 5          | 2375.0         | 475.0        | NULL                | NULL                |
+------------+-------------+---------+------------+----------------+--------------+---------------------+---------------------+

=> Insights: Truy vấn này phân tích hiệu suất doanh thu hàng tháng theo từng khu vực. Kết quả giúp xác định khu vực nào đang tăng trưởng, khu vực nào đang suy giảm và mức độ thay đổi doanh thu so với tháng trước.

4.

SELECT 
    product_name,
    COUNT(*) as sales_count,
    SUM(quantity) as total_quantity_sold,
    SUM(revenue) as total_revenue,
    AVG(unit_price) as avg_unit_price,
    PERCENTILE_APPROX(revenue, 0.5) as median_revenue,
    COLLECT_LIST(region) as regions_sold
FROM sales_fact
GROUP BY product_name
ORDER BY total_revenue DESC;


+-----------------------------+--------------+----------------------+----------------+-----------------+-----------------+--------------------+
|        product_name         | sales_count  | total_quantity_sold  | total_revenue  | avg_unit_price  | median_revenue  |    regions_sold    |
+-----------------------------+--------------+----------------------+----------------+-----------------+-----------------+--------------------+
| Laptop                      | 2            | 3                    | 3600.0         | 1200.0          | 1200.0          | ["North","North"]  |
| Phone                       | 1            | 3                    | 2400.0         | 800.0           | 2400.0          | ["North"]          |
| Tablet                      | 1            | 2                    | 1000.0         | 500.0           | 1000.0          | ["South"]          |
| Camera                      | 1            | 1                    | 600.0          | 600.0           | 600.0           | ["North"]          |
| Headphones                  | 1            | 4                    | 600.0          | 150.0           | 600.0           | ["South"]          |
| Monitor                     | 1            | 1                    | 350.0          | 350.0           | 350.0           | ["South"]          |
| Printer                     | 1            | 1                    | 300.0          | 300.0           | 300.0           | ["South"]          |
| Keyboard                    | 1            | 3                    | 225.0          | 75.0            | 225.0           | ["North"]          |
| Mouse                       | 1            | 5                    | 125.0          | 25.0            | 125.0           | ["South"]          |
| john.smith@company.com      | 1            | NULL                 | NULL           | 75000.0         | NULL            | []                 |
| jane.doe@company.com        | 1            | NULL                 | NULL           | 65000.0         | NULL            | []                 |
| henry.taylor@company.com    | 1            | NULL                 | NULL           | 62000.0         | NULL            | []                 |
| grace.moore@company.com     | 1            | NULL                 | NULL           | 78000.0         | NULL            | []                 |
| frank.wilson@company.com    | 1            | NULL                 | NULL           | 72000.0         | NULL            | []                 |
| eve.miller@company.com      | 1            | NULL                 | NULL           | 67000.0         | NULL            | []                 |
| diana.davis@company.com     | 1            | NULL                 | NULL           | 82000.0         | NULL            | []                 |
| charlie.brown@company.com   | 1            | NULL                 | NULL           | 60000.0         | NULL            | []                 |
| bob.johnson@company.com     | 1            | NULL                 | NULL           | 80000.0         | NULL            | []                 |
| alice.williams@company.com  | 1            | NULL                 | NULL           | 70000.0         | NULL            | []                 |
| NULL                        | 71           | NULL                 | NULL           | NULL            | NULL            | []                 |
+-----------------------------+--------------+----------------------+----------------+-----------------+-----------------+--------------------+

=> Insights: Kết quả truy vấn cho thấy Laptop là sản phẩm tạo ra doanh thu cao nhất với tổng doanh thu đạt 3.600, tiếp theo là Phone với 2.400. Mặc dù Phone chỉ bán được 3 sản phẩm nhưng vẫn tạo ra doanh thu rất cao, cho thấy đây là mặt hàng có giá trị trên mỗi đơn vị bán lớn. Ngược lại, Mouse có số lượng bán cao nhất (5 sản phẩm) nhưng chỉ tạo ra 125 doanh thu, cho thấy đây là sản phẩm có giá trị thấp, phù hợp với chiến lược bán theo số lượng thay vì doanh thu.

Xét theo khu vực, các sản phẩm thuộc North tạo ra tổng doanh thu khoảng 6.825, cao hơn đáng kể so với South với khoảng 2.375. Điều này cho thấy North hiện là thị trường đóng góp doanh thu chính của doanh nghiệp.

5.


+----------+-------------+---------------+----------+-----------------+-------------+-------------------+-------------------------+----------------------+
| sale_id  |  sale_date  | product_name  | revenue  |  employee_name  | department  | daily_sales_rank  | employee_total_revenue  | product_avg_revenue  |
+----------+-------------+---------------+----------+-----------------+-------------+-------------------+-------------------------+----------------------+
| 2009     | 2024-01-23  | Camera        | 600.0    | Alice Williams  | Sales       | 1                 | 6825.0                  | 600.0                |
| 2008     | 2024-01-22  | Headphones    | 600.0    | Frank Wilson    | Sales       | 1                 | 2375.0                  | 600.0                |
| 2003     | 2024-01-17  | Keyboard      | 225.0    | Alice Williams  | Sales       | 1                 | 6825.0                  | 225.0                |
| 2001     | 2024-01-15  | Laptop        | 2400.0   | Alice Williams  | Sales       | 1                 | 6825.0                  | 1800.0               |
| 2005     | 2024-01-19  | Laptop        | 1200.0   | Alice Williams  | Sales       | 1                 | 6825.0                  | 1800.0               |
| 2004     | 2024-01-18  | Monitor       | 350.0    | Frank Wilson    | Sales       | 1                 | 2375.0                  | 350.0                |
| 2002     | 2024-01-16  | Mouse         | 125.0    | Frank Wilson    | Sales       | 1                 | 2375.0                  | 125.0                |
| 2007     | 2024-01-21  | Phone         | 2400.0   | Alice Williams  | Sales       | 1                 | 6825.0                  | 2400.0               |
| 2010     | 2024-01-24  | Printer       | 300.0    | Frank Wilson    | Sales       | 1                 | 2375.0                  | 300.0                |
| 2006     | 2024-01-20  | Tablet        | 1000.0   | Frank Wilson    | Sales       | 1                 | 2375.0                  | 1000.0               |
+----------+-------------+---------------+----------+-----------------+-------------+-------------------+-------------------------+----------------------+


=> Insights: 
1. Hiệu suất nhân viên
- Alice Williams tạo ra tổng doanh thu 6,825$, cao hơn Frank Wilson (2,375$).
- Alice là nhân viên có hiệu suất bán hàng tốt nhất.

2. Sản phẩm doanh thu cao
- Laptop và Phone có doanh thu cao nhất với 2,400$.
- Đây là hai sản phẩm đóng góp doanh thu lớn nhất.

3. Xếp hạng doanh thu theo ngày
- Tất cả các giao dịch trong kết quả đều có daily_sales_rank = 1.
- Điều này cho thấy mỗi giao dịch là giao dịch doanh thu cao nhất trong ngày tương ứng.

4. Doanh thu trung bình sản phẩm
- Laptop có doanh thu trung bình 1,800$.
- Giao dịch Laptop ngày 2024-01-15 đạt 2,400$, cao hơn mức trung bình.
- Giao dịch Laptop ngày 2024-01-19 đạt 1,200$, thấp hơn mức trung bình.

6. 

CREATE TABLE sales_partitioned (
    sale_id INT,
    employee_id INT,
    customer_id INT,
    product_name STRING,
    quantity INT,
    unit_price DOUBLE,
    revenue DOUBLE,
    sale_date DATE
)
PARTITIONED BY (region STRING, sale_year INT)
STORED AS PARQUET;

SET hive.exec.dynamic.partition = true;
SET hive.exec.dynamic.partition.mode = nonstrict;

=> Partition giúp chia data theo nhóm và khi cần tìm theo nhóm nào đó sẽ lấy data ra nhanh khi cần. Nó giống như
1 đống hồ sơ gồm nhiều loại tài liệu và khi ta cần tài liệu theo region = North thì cần phải scan qua từng tờ
thì mới lấy ra được region = North. Vì thế Hive giúp partition data theo column yêu cầu.
"SET hive.exec.dynamic.partition = true;
SET hive.exec.dynamic.partition.mode = nonstrict;" => 2 dòng này có mục đích lần lượt là mỗi khi cần insert data
thì ta cần phải chỉ rõ cần partition column nào vì thế khá mệt. Dòng đầu giúp partition tạo tự động và dòng sau 
giúp ko cần phải set giá trị cố định mà Hive sẽ tự xem data xem value đó đã có partition chưa nếu chưa nó sẽ tự 
partition

