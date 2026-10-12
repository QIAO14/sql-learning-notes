# 01 基础查询与多表连接

## 1. 基础查询

```sql
-- 查询所有列
SELECT * FROM employee;

-- 查询指定列 + 条件 + 排序
SELECT name, salary
FROM employee
WHERE salary > 5000
ORDER BY salary DESC;

-- 去重
SELECT DISTINCT dept_id FROM employee;

-- 模糊匹配
SELECT name FROM employee WHERE name LIKE '张%';
```

## 2. 多表连接（JOIN）

```sql
-- INNER JOIN：只返回两表匹配的行
SELECT e.name, d.dept_name
FROM employee e
INNER JOIN dept d ON e.dept_id = d.id;

-- LEFT JOIN：左表全保留，右表无匹配显示 NULL
SELECT e.name, d.dept_name
FROM employee e
LEFT JOIN dept d ON e.dept_id = d.id;

-- RIGHT JOIN：右表全保留，左表无匹配显示 NULL
SELECT e.name, d.dept_name
FROM employee e
RIGHT JOIN dept d ON e.dept_id = d.id;
```

### 连接类型对比

| 连接类型 | 结果 |
|---|---|
| INNER JOIN | 只返回匹配的行 |
| LEFT JOIN | 左表全部 + 右表匹配 |
| RIGHT JOIN | 右表全部 + 左表匹配 |

### 连接多个表

```sql
SELECT e.name, d.dept_name, p.project_name
FROM employee e
JOIN dept d ON e.dept_id = d.id
JOIN project p ON e.id = p.owner_id;
```

## 3. 常见坑

- JOIN 条件写错会导致笛卡尔积（行数爆炸），连接前先确认关联字段
- 多表连接字段有重名时，必须用表别名区分（e.name / d.name）
