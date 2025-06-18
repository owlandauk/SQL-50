# SQL-50

# Select:

## [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/description/?envType=study-plan-v2&envId=top-sql-50)

- In SQL, `NULL` means "unknown".
- Any comparison with `NULL` (`=`, `!=`, `<`, `>`, etc.) results in `UNKNOWN`, **not TRUE**.

## [1148. Article Views I](https://leetcode.com/problems/article-views-i/)

The correct function to get the length of a string in MySQL is `CHAR_LENGTH()` (or `LENGTH()`, but `CHAR_LENGTH()` is better for character counts, especially with multibyte characters).

# Basic Joins:

## [1378. Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)

| JOIN Type | Keeps all rows from... | Fills `NULL` for... |
| --- | --- | --- |
| `INNER JOIN` | Only matched rows | No unmatched rows shown |
| `LEFT JOIN` | Left table | Right table when there's no match |
| `RIGHT JOIN` | Right table | Left table when there's no match |

## [1581. Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/)

✅ Difference Between `WHERE` and `HAVING` in SQL

| Feature | `WHERE` | `HAVING` |
| --- | --- | --- |
| **When it works** | Before `GROUP BY` (filters **rows**) | After `GROUP BY` (filters **groups**) |
| **Uses Aggregates?** | ❌ Cannot use aggregate functions like `SUM()`,COUNT() | ✅ Can use aggregate functions like `SUM()` |
| **Main use case** | Filter individual rows before grouping | Filter grouped results after aggregation |

`CASE WHEN transaction_id IS NULL THEN 1 END`：return 1 only if it is NULL ,otherwise it is NULL (not counted)

## [197. Rising Temperature](https://leetcode.com/problems/rising-temperature/)

The `DATEDIFF()` function in **MySQL** is used to **calculate the number of days between two dates**.

---

### ✅ Syntax:

```
DATEDIFF(date1, date2)=1  (1 day)
```

## [1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/)

We're going to use a **CROSS JOIN**

which is like making every possible pair of rows from two tables.

![image.png](image.png)

📝 MySQL GROUP BY 多列

```sql
SELECT column1, column2, AGG_FUNC(column3)
FROM table_name
GROUP BY column1, column2;
```

### ✅ 多列分组的作用：

当你希望按照多个字段的组合来分组数据时，可以使用 `GROUP BY column1, column2`。系统会把具有相同 column1 和 column2 值的记录分为一组，然后对每组使用聚合函数

## [1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)

`AVG(condition) is an attack method that calculates the attack that satisfies a certain condition;
IFNULL(..., 0) ensures that a default value is given even if there is no data;`

```sql
IFNULL(AVG(C.action = 'confirmed'), 0)
```

# Basic Aggregate Functions

## [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)

| Symbol | Meaning |
| --- | --- |
| `=` | Equal to |
| `<>` | Not equal to |
| `!=` | Not equal to |

## [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/)

A **BETWEEN** B **AND** C

```sql
AND U.purchase_date BETWEEN P.start_date AND P.end_date
```

## [1211. Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/)

**CASE WHEN** A<B **THEN** C **ELSE** D **END**

```sql
CASE WHEN rating < 3 THEN 1 ELSE 0 END
```

## [1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/)

**DATE_FORMAT**

| Code | Output | Meaning |
| --- | --- | --- |
| `%m` | `01`, `02`, ..., `12` | Month as number |
| `%M` | `January`, ..., `December` | Full month name |
| `%b` | `Jan`, `Feb`, ..., `Dec` | Abbreviated name |
| `%Y` | `2024` | 4-digit year |

```sql
DATE_FORMAT(trans_date,'%Y-%m') 
```

## [550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT
  ROUND(
    COUNT(DISTINCT player_id) /
    (SELECT COUNT(DISTINCT player_id) FROM Activity),
    2
  ) AS fraction
FROM
  Activity
WHERE
  DATEDIFF(event_date, (
    SELECT MIN(a2.event_date)
    FROM Activity a2
    WHERE a2.player_id = Activity.player_id
  )) = 1;
```

# **Advanced Select and Joins**

## [**180. Consecutive Numbers**](https://leetcode.com/problems/consecutive-numbers/description/?envType=study-plan-v2&envId=top-sql-50)

### 📌 `LEAD(expr, offset)` 是 **窗口函数（window function）**，它的作用是：

> 从“当前行”往 “后面第 offset 行” 取一个值。
> 

`OVER()` 表示**这个窗口函数的作用范围（窗口）**。

你可以在里面加排序或分组，比如：

```sql
with cte as (
    select num,
    lead(num,1) over() num1,
    lead(num,2) over() num2
    from logs

)

select distinct num ConsecutiveNums from cte where (num=num1) and (num=num2)
```
