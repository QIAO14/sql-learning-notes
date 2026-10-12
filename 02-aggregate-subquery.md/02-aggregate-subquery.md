# 02 聚合分组、子查询与 NULL 处理

## 1. 聚合函数

```sql
-- 常用聚合函数
SELECT
  COUNT(*) AS 总人数,
    AVG(salary) AS 平均工资,
      MAX(salary) AS 最高工资,
        MIN(salary) AS 最低工资,
          SUM(salary) AS 工资总和
          FROM employee;
          ```

          ## 2. 分组聚合（GROUP BY）

          ```sql
          -- 按部门统计人数和平均工资
          SELECT dept_id, COUNT(*) AS cnt, AVG(salary) AS avg_salary
          FROM employee
          GROUP BY dept_id;

          -- 分组后过滤（HAVING）
          SELECT dept_id, AVG(salary) AS avg_salary
          FROM employee
          GROUP BY dept_id
          HAVING AVG(salary) > 6000;
          ```

          ### WHERE vs HAVING

          | | WHERE | HAVING |
          |---|---|---|
          | 执行时机 | 分组前过滤行 | 分组后过滤组 |
          | 能否用聚合函数 | 不能 | 可以 |
          | 例子 | WHERE salary > 5000 | HAVING COUNT(*) > 3 |

          ## 3. 子查询

          ```sql
          -- 标量子查询：查询比部门平均工资高的员工
          SELECT name, salary, dept_id
          FROM employee e
          WHERE salary > (
            SELECT AVG(salary) FROM employee WHERE dept_id = e.dept_id
            );

            -- 用 IN 的子查询
            SELECT name FROM employee
            WHERE dept_id IN (SELECT id FROM dept WHERE location = '重庆');
            ```

            ## 4. NULL 处理

            ```sql
            -- NULL 不能用 = 判断，必须用 IS NULL / IS NOT NULL
            SELECT * FROM employee WHERE email IS NULL;

            -- 填充默认值
            SELECT name, IFNULL(phone, '未填写') FROM employee;

            -- COALESCE 多值取第一个非空
            SELECT name, COALESCE(phone, email, '无联系方式') FROM employee;
            ```

            ### NULL 的坑
            - `NULL = NULL` 结果是 NULL（未知），不是 true
            - 聚合函数 COUNT(列名) 会跳过 NULL，COUNT(*) 不会
            - 用 COUNT(字段) 统计该字段非空行数
