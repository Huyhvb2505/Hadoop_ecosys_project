1. Tạo database nếu nó chưa tồn tại.
    CREATE DATABASE IF NOT EXISTS company_dw;
    USE company_dw;
2. chi tiết về table
    DESCRIBE employees_ext;
    +--------------+------------+----------+
    |   col_name   | data_type  | comment  |
    +--------------+------------+----------+
    | employee_id  | int        |          |
    | first_name   | string     |          |
    | last_name    | string     |          |
    | email        | string     |          |
    | department   | string     |          |
    | salary       | double     |          |
    | hire_date    | date       |          |
    +--------------+------------+----------+
    7 rows selected (0.082 seconds)

3.
    describe employees_managed;
    +-------------------+------------+----------+
    |     col_name      | data_type  | comment  |
    +-------------------+------------+----------+
    | employee_id       | int        |          |
    | first_name        | string     |          |
    | last_name         | string     |          |
    | email             | string     |          |
    | department        | string     |          |
    | salary            | double     |          |
    | hire_date         | date       |          |
    | years_experience  | int        |          |
    +-------------------+------------+----------+
    8 rows selected (0.074 seconds)


    1. Load data from external table to managed table with transformation

4. load data from external table -> managed table ( external(metadata) -> location path(/user/student/input/) -> employees.csv -> read data -> load to managed table)
    INSERT INTO employees_managed
    . . . . . . . . . . . . . . . .> SELECT
    . . . . . . . . . . . . . . . .>     employee_id,
    . . . . . . . . . . . . . . . .>     first_name,
    . . . . . . . . . . . . . . . .>     last_name,
    . . . . . . . . . . . . . . . .>     email,
    . . . . . . . . . . . . . . . .>     department,
    . . . . . . . . . . . . . . . .>     salary,
    . . . . . . . . . . . . . . . .>     hire_date,
    . . . . . . . . . . . . . . . .>     CAST(DATEDIFF(CURRENT_DATE, hire_date) / 365 AS INT) as years_experience
    . . . . . . . . . . . . . . . .> FROM employees_ext;
    WARNING: Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
    No rows affected (6.825 seconds)

    