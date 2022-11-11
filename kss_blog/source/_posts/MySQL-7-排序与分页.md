---
title: MySQL-7-排序与分页
date: 2022-10-27 19:51:07
tags: MySQL
categories: MySQL
---

## 排序数据

### 排序规则

* **使用ORDER BY子句排序**
  * **ASC(ascend):升序**
  * **DESC(descend):降序**
* **ORDER BY子句在SELECT语句的结尾**

### 单列排序

```sql
mysql> select employee_id , last_name , salary 
    -> from employees
    -> order by salary asc;
+-------------+-------------+----------+
| employee_id | last_name   | salary   |
+-------------+-------------+----------+
|         132 | Olson       |  2100.00 |
|         128 | Markle      |  2200.00 |
|         136 | Philtanker  |  2200.00 |
|         127 | Landry      |  2400.00 |
|         135 | Gee         |  2400.00 |
|         119 | Colmenares  |  2500.00 |
|         131 | Marlow      |  2500.00 |
|         140 | Patel       |  2500.00 |
|         144 | Vargas      |  2500.00 |
```

#### 使用列的别名进行排序

```sql
SELECT employee_id,salary,salary * 12 annual_sal #as可以省略
FROM employees
ORDER BY annual_sal;
```

> **notice:列的别名只能在ORDER BY 中使用，不能在WHERE中使用，会报错。**
>
> **强调格式： WHERE 需要声明在FROM后，ORDER BY之前。**

### 多列排序(二级排序)

```
mysql> select last_name , department_id , salary
    -> from employees
    -> order by department_id , salary desc;
+-------------+---------------+----------+
| last_name   | department_id | salary   |
+-------------+---------------+----------+
| Grant       |          NULL |  7000.00 |
| Whalen      |            10 |  4400.00 |
| Hartstein   |            20 | 13000.00 |
| Fay         |            20 |  6000.00 |
| Raphaely    |            30 | 11000.00 |
| Khoo        |            30 |  3100.00 |
| Baida       |            30 |  2900.00 |
| Tobias      |            30 |  2800.00 |
| Himuro      |            30 |  2600.00 |
| Colmenares  |            30 |  2500.00 |
| Mavris      |            40 |  6500.00 |
| Fripp       |            50 |  8200.00 |
| Weiss       |            50 |  8000.00 |
```

* **可以使用不在SELECT列表中的排序**
* **在对多列进行排序的时候，首先排序的第一列必须有相同的列值，才会对第二列进行排序。如果第一列数据中所有值都是唯一的，将不再对第二列进行排序**

## 分页

### 实现规则

* **分页原理**

**所谓分页显示，就是将数据库中的结果集，一段一段显示出来需要的条件**

* **MySQL中使用LIMIT实现分页**
* **格式：**

```sql
LIMIT [位置偏移量] 行数
```

* **example**

```sql
mysql> select employee_id , first_name , last_name  from employees limit 0,10;
+-------------+------------+-----------+
| employee_id | first_name | last_name |
+-------------+------------+-----------+
|         100 | Steven     | King      |
|         101 | Neena      | Kochhar   |
|         102 | Lex        | De Haan   |
|         103 | Alexander  | Hunold    |
|         104 | Bruce      | Ernst     |
|         105 | David      | Austin    |
|         106 | Valli      | Pataballa |
|         107 | Diana      | Lorentz   |
|         108 | Nancy      | Greenberg |
|         109 | Daniel     | Faviet    |
+-------------+------------+-----------+
10 rows in set (0.00 sec)
```

> **MySQL8.0中可以使用"LIMIT 3 OFFSET 4",意思是显示从第五条记录开始后面的3条记录，和"LIMIT 4,3"返回的结果相同。**

* **分页显示公式：（第几页-1）* 每页条数，每页条数**

```sql
SELECT * FROM table
LIMIT (PageNo - 1)*PageSize,PageSize;
```

* **注意：LIMIT 子句必须放在整个SELECT语句的最后**
* **使用LIMIT的好处**
  * **减少数据表的网络传输量**
  * **提升查询效率**

## 扩展

**在不同的 DBMS 中使用的关键字可能不同。在 MySQL、PostgreSQL、MariaDB 和 SQLite 中使用 LIMIT 关 键字，而且需要放到 SELECT 语句的最后面。**

* **如果是 SQL Server 和 Access，需要使用 TOP 关键字，比如：**

```sql
SELECT TOP 5 name, hp_max FROM heros ORDER BY hp_max DESC
```

* **如果是 DB2，使用 FETCH FIRST 5 ROWS ONLY 这样的关键字：**

```sql
SELECT name, hp_max FROM heros ORDER BY hp_max DESC FETCH FIRST 5 ROWS ONLY
```

* **如果是 Oracle，你需要基于 ROWNUM 来统计行数：**

```sql
SELECT rownum,last_name,salary FROM employees WHERE rownum < 5 ORDER BY salary DESC;
```

**需要说明的是，这条语句是先取出来前 5 条数据行，然后再按照 hp_max 从高到低的顺序进行排序。但 这样产生的结果和上述方法的并不一样。我会在后面讲到子查询，你可以使用**

```sql
SELECT last_name,salary
FROM employees
ORDER BY salary DESC)
WHERE rownum < 10;
```

**得到与上述方法一致的结果。**
