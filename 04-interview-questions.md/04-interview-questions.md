# 04 面试高频题

## 1. 查询每个部门工资最高的员工

```sql
SELECT name, dept_id, salary
FROM (
  SELECT *, DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) rk
    FROM employee
    ) t
    WHERE rk = 1;
    ```

    ## 2. 查询第 N 高的薪水（如第2高）

    ```sql
    SELECT DISTINCT salary
    FROM (
      SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) rk
        FROM employee
        ) t
        WHERE rk = 2;
        ```

        ## 3. 查询比所在部门平均工资高的员工

        ```sql
        SELECT name, salary, dept_id
        FROM employee e
        WHERE salary > (
          SELECT AVG(salary) FROM employee WHERE dept_id = e.dept_id
          );
          ```

          ## 4. 每个用户第二次下单的金额

          ```sql
          SELECT user_id, order_amount
          FROM (
            SELECT *, ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY order_time) rn
              FROM orders
              ) t
              WHERE rn = 2;
              ```

              ## 5. 连续登录 N 天的用户（进阶）

              思路：用日期减去行号得到连续组的标记

              ```sql
              SELECT user_id
              FROM (
                SELECT user_id, login_date,
                         DATE_SUB(login_date, INTERVAL ROW_NUMBER()
                                    OVER (PARTITION BY user_id ORDER BY login_date) DAY) AS grp
                                      FROM login_log
                                      ) t
                                      GROUP BY user_id, grp
                                      HAVING COUNT(*) >= 3;
                                      ```

                                      ## 6. 排名差分析（重庆经济项目实战）

                                      排名差 = 人均排名 - GDP总量排名：
                                      - 负值 = 人均排名靠前（藏富于民，如渝中）
                                      - 正值 = 总量靠前但人均被摊薄（人口多，如渝北）

                                      ```sql
                                      -- 注意：RANK() 返回 BIGINT UNSIGNED，相减为负会溢出，必须转有符号
                                      SELECT district,
                                             RANK() OVER (ORDER BY gdp DESC) AS gdp_rk,
                                                    RANK() OVER (ORDER BY per_capita_gdp DESC) AS per_rk,
                                                           CAST(RANK() OVER (ORDER BY per_capita_gdp DESC) AS SIGNED)
                                                                    - CAST(RANK() OVER (ORDER BY gdp DESC) AS SIGNED) AS diff
                                                                    FROM cq_economy;
                                                                    ```
