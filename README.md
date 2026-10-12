# SQL 学习笔记

个人学习 SQL 的记录，从零基础到能独立完成数据分析查询。内容以 MySQL 为主。

## 仓库结构

| 文件 | 内容 |
|---|---|
| 01-basics-join.md | 基础查询、JOIN 多表连接 |
| 02-aggregate-subquery.md | 聚合分组、子查询、NULL 处理 |
| 03-window-functions.md | 窗口函数（ROW_NUMBER / RANK / DENSE_RANK） |
| 04-interview-questions.md | 面试高频题（部门最高工资、第N高薪水等） |

## 核心知识点速览

### JOIN 三种连接
- INNER JOIN：只返回两表匹配的行
- LEFT JOIN：左表全部保留，右表无匹配显示 NULL
- RIGHT JOIN：右表全部保留，左表无匹配显示 NULL

### WHERE vs HAVING
- WHERE：分组前过滤行，不能用聚合函数
- HAVING：分组后过滤组，可以用聚合函数

### 三种排名函数
| 函数 | 并列处理 | 例子 |
|---|---|---|
| ROW_NUMBER() | 不管并列，唯一序号 | 1,2,3,4 |
| RANK() | 并列占名次，后面跳号 | 1,1,3,4 |
| DENSE_RANK() | 并列占名次，不跳号 | 1,1,2,3 |

### 窗口函数 vs GROUP BY
- 窗口函数不减少行数，每行保留原始信息同时做聚合
- GROUP BY 会把多行合并成一行

### NULL 处理
- NULL 不能用 = 判断，要用 IS NULL / IS NOT NULL
- 用 IFNULL() 或 COALESCE() 填充默认值

## 后续计划
- 索引与执行计划
- 事务 ACID
- 存储过程
- LOAD DATA 数据导入导出
- 
