# 03 窗口函数

## 1. 什么是窗口函数

窗口函数不减少行数，每一行都保留原始信息，同时可以拿到聚合结果。适合做排名、累计、同环比等分析。

```sql
-- 基本结构：函数() OVER (PARTITION BY 分组 ORDER BY 排序)
SELECT name, dept_id, salary,
       RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rk
       FROM employee;
       ```

       ## 2. 三种排名函数

       ```sql
       SELECT name, salary,
              ROW_NUMBER() OVER (ORDER BY salary DESC) AS rn,
                     RANK()       OVER (ORDER BY salary DESC) AS rk,
                            DENSE_RANK() OVER (ORDER BY salary DESC) AS drk
                            FROM employee;
                            ```

                            | 函数 | 并列处理 | 例子 |
                            |---|---|---|
                            | ROW_NUMBER() | 不管并列，唯一序号 | 1,2,3,4 |
                            | RANK() | 并列占名次，后面跳号 | 1,1,3,4 |
                            | DENSE_RANK() | 并列占名次，不跳号 | 1,1,2,3 |

                            ## 3. 聚合窗口函数

                            ```sql
                            -- 每行显示部门总人数和平均工资（不合并行）
                            SELECT name, dept_id, salary,
                                   COUNT(*) OVER (PARTITION BY dept_id) AS dept_cnt,
                                          AVG(salary) OVER (PARTITION BY dept_id) AS dept_avg
                                          FROM employee;

                                          -- 累计求和（按工资排序累加）
                                          SELECT name, salary,
                                                 SUM(salary) OVER (ORDER BY salary DESC) AS cum_sum
                                                 FROM employee;
                                                 ```

                                                 ## 4. 窗口函数 vs GROUP BY

                                                 | | 窗口函数 | GROUP BY |
                                                 |---|---|---|
                                                 | 行数 | 不减少，每行保留 | 多行合并成一行 |
                                                 | 用途 | 排名、累计、每行对比 | 汇总统计 |
                                                 | 例子 | 部门内工资排名 | 部门平均工资 |

                                                 ## 5. 常用场景

                                                 ### 每组取前 N 名

                                                 ```sql
                                                 SELECT name, dept_id, salary
                                                 FROM (
                                                   SELECT *, DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) rk
                                                     FROM employee
                                                     ) t
                                                     WHERE rk <= 3;
                                                     ```

                                                     ### 与上一行对比（LAG）

                                                     ```sql
                                                     SELECT name, salary,
                                                            LAG(salary, 1) OVER (ORDER BY salary) AS prev_salary,
                                                                   salary - LAG(salary, 1) OVER (ORDER BY salary) AS diff
                                                                   FROM employee;
                                                                   ```
