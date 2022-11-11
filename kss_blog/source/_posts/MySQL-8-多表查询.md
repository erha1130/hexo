---
title: 多表查询
date: 2022-10-28 14:09:04
tags: MySQL
categories: MySQL
---

# 多表查询

**多表查询，也称关联查询，指两个或多个表一起完成查询操作。**

**前提条件：这些一起查询的表之间是有关系的（一对一、一对多），它们之间一定是有关联字段，这个关联字段可能建立了外键，也可能没有建立外键。比如：员工表和部门表，这两个表依靠“部门编号”进行关联。**

### 笛卡尔积（或交叉连接）的理解

笛卡尔乘积是一个数学运算。假设我有两个集合 X 和 Y，那么 X 和 Y 的笛卡尔积就是 X 和 Y 的所有可能 组合，也就是第一个对象来自于 X，第二个对象来自于 Y 的所有可能。组合的个数即为两个集合中元素 个数的乘积数。

**SQL92中，笛卡尔积也称为 交叉连接 ，英文是 CROSS JOIN 。在 SQL99 中也是使用 CROSS JOIN表示交 叉连接。**

```sql
#出现笛卡尔积的错误
#错误的原因：缺少了多表的连接条件
#错误的实现方式：每个员工都与每个部门匹配了一遍。
SELECT employee_id,department_name
FROM employees,departments;  #查询出2889条记录
```

* **笛卡尔积的错误会在下面条件下产生：**
  * **省略多个表的连接条件（或关联条件）**
  * **连接条件(或关联条件)无效**
  * **所有表中的所有行互相连接**
* **为了避免笛卡尔积，可以在WHERE加入有效的连接条件**
* **加入连接条件后，查询语法：**

```sql
SELECT table1.column, table2.column
FROM   table1,table2
WHERE  table1.column1 = table2.column2; #连接条件
```

* **正确写法：**

```sql
#案例：查询员工的姓名及其部门名称
SELECT last_name, department_name
FROM employees, departments
WHERE employees.department_id = departments.department_id;
```

* **在表中有相同列时，在列名之前加上表名前缀。**

* **建议：从sql优化的角度，建议多表查询时，每个字段前都指明其所在的表。**
* **可以在FROM中给表起别名，在SELECT和WHERE中使用表的别名。如果给表起了别名，一旦在SELECT或WHERE中使用表名的话，则必须使用表的别名，而不能再使用表的原名。**

{% note success modern %}
**结论：如果有n个表实现多表的查询，则需要至少n-1个连接条件**
{% endnote %}

### 多表查询分类讲解

#### 等值连接vs非等值连接

##### 非等值连接

```sql
SELECT e.last_name,e.salary,j.grade_level
FROM employees e,job_grades j
#where e.`salary` between j.`lowest_sal` and j.`highest_sal`;
WHERE e.`salary` >= j.`lowest_sal` AND e.`salary` <= j.`highest_sal`;
```

#### 自连接 vs 非自连接

![linux45.png](https://s2.loli.net/2022/10/30/t8hJpgw5VqlxZF7.png)

* **当table1和table2本质上是同一张表，只是用取别名的方式虚拟成两张表以代表不同的意义。然后两 个表再进行内连接，外连接等查询。**

```sql
#自连接的例子：
#练习：查询员工id,员工姓名及其管理者的id和姓名
SELECT emp.employee_id,emp.last_name,mgr.employee_id,mgr.last_name
FROM employees emp ,employees mgr
WHERE emp.`manager_id` = mgr.`employee_id`;
```



#### 内连接 vs 外连接

**除了查询满足条件的记录以外，外连接还可以查询某一方不满足条件的记录。**

* **内连接：合并具有同一列的两个以上的表的行，结果集中不包含一个表与另一个表不匹配的行**
* **外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回左（或右）表中不满足条件的行，这种连接称为左（或右）外连接。没有匹配的行时，结果表中相应的列为空(null).**
* **如果是左外连接，则连接条件中左边的表也称为主表，右边的表称为从表。**
* **如果是右外连接，则连接条件中右边的表也称为主表，左边的表称为从表。**

#### SQL99语法实现多表查询

* **使用JOIN...ON子句创建连接的语法结构：**

```sql
SELECT table1.column, table2.column,table3.column
FROM table1 
JOIN table2 ON table1 和 table2 的连接条件
JOIN table3 ON table2 和 table3 的连接条件
```

* **语法说明**
  * **可以使用ON子句指定额外的连接条件**
  * **这个连接条件是与其他条件分开的。**
  * **ON子句使语句具有更高的易读性**
  * **关键字JOIN、INNER JOIN、CROSS JOIN的含义是一样的，都表示内连接**

#### 内连接(INNER JOIN)的实现

* **语法：**

```sql
SELECT 字段列表
FROM A表 INNER JOIN B表
ON 连接条件
WHERE 等其他子句;
```

* **example:**

```sql
mysql> SELECT last_name,department_name,city
    -> FROM employees e JOIN departments d
    -> ON e.`department_id` = d.`department_id`
    -> JOIN locations l
    -> ON d.`location_id` = l.`location_id`;
```

#### 外连接(OUTER JOIN)的实现

##### 左外连接(LEFT OUTER JOIN)（包括重复的部分）

* **语法：**

```sql
#实现查询结果是A
SELECT 字段列表
FROM A表 LEFT JOIN B表
ON 关联条件
WHERE 等其他子句;
```

##### 右外连接(RIGHT OUTER JOIN)

* **语法：**

```sql
#实现查询结果是B
SELECT 字段列表
FROM A表 RIGHT JOIN B表
ON 关联条件
WHERE 等其他子句;
```

* **example**

```sql
SELECT e.last_name, e.department_id, d.department_name
FROM employees e
RIGHT OUTER JOIN departments d
ON (e.department_id = d.department_id) ;
```

**{% tip success %}需要注意的是，LEFT JOIN 和 RIGHT JOIN 只存在于 SQL99 及以后的标准中，在 SQL92 中不存在， 只能用 (+) 表示。{% endtip %}**

##### 满外连接(FULL OUTER JOIN)

* **满外连接的结果 = 左右表匹配的数据 + 左表没有匹配到的数据 + 右表没有匹配到的数据。**
* **SQL99是支持满外连接的。使用FULL JOIN 或 FULL OUTER JOIN来实现。**
* **需要注意的是，MySQL不支持FULL JOIN，但是可以用 LEFT JOIN UNION RIGHT join代替。**

#### UNION的使用

**{% span red, 合并查询结果 %}  利用UNION关键字，可以给出多条SELECT语句，并将它们的结果组合成单个结果集。合并 时，两个表对应的列数和数据类型必须相同，并且相互对应。各个SELECT语句之间使用UNION或UNION ALL关键字分隔。**

* **语法格式：**

```sql
SELECT column,... FROM table1
UNION [ALL]
SELECT column,... FROM table2
```

* **UNION操作符**

![mysql19.png](https://s2.loli.net/2022/10/31/yQZa9pBILjrKwAm.png)

**返回并集**

* **UNION ALL操作符**

<img src="https://s2.loli.net/2022/10/31/FUOi7QI8d3rVcs6.png" alt="mysql20.png" style="zoom:50%;" />

**UNION ALL操作符返回两个查询的结果集的并集。对于两个结果集的重复部分，不去重。**

**{% tip success %}**

**注意：执行UNION ALL语句时所需要的资源比UNION语句少。如果明确知道合并数据后的结果数据**
**不存在重复数据，或者不需要去除重复的数据，则尽量使用UNION ALL语句，以提高数据查询的效**
**率。{% endtip %}**

* **example**

**查询部门编号>90或邮箱包含a的员工信息**

```sql
#方式1
SELECT * FROM employees WHERE email LIKE '%a%' OR department_id>90;
```

```sql
#方式2
SELECT * FROM employees WHERE email LIKE '%a%'
UNION
SELECT * FROM employees WHERE department_id>90;
```

### 7种SQL JOINS的实现

![mysql21.png](https://s2.loli.net/2022/10/31/z5aE9ibHhOB4QsR.png)

####  代码实现

```sql
#中图：内连接 A∩B
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`;
```

```sql
#左上图：左外连接
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`;
```

```sql
#右上图：右外连接
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;
```

```sql
#左中图：A - A∩B
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
```

```sql
#右中图：B-A∩B
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL
```

```sql
#左下图：满外连接
# 左中图 + 右上图 A∪B
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL #没有去重操作，效率高
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;
```

```sql
#右下图
#左中图 + 右中图 A ∪B- A∩B 或者 (A - A∩B) ∪ （B - A∩B）
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL
```

####  语法格式小结

* **左中图**

```sql
#实现A - A∩B
select 字段列表
from A表 left join B表
on 关联条件
where 从表关联字段 is null and 等其他子句;
```

* **右中图**

```sql
#实现B - A∩B
select 字段列表
from A表 right join B表
on 关联条件
where 从表关联字段 is null and 等其他子句;
```

* **左下图**

```sql
#实现查询结果是A∪B
#用左外的A，union 右外的B
select 字段列表
from A表 left join B表
on 关联条件
where 等其他子句
union
select 字段列表
from A表 right join B表
on 关联条件
where 等其他子句;
```

* **右下图**

```sql
#实现A∪B - A∩B 或 (A - A∩B) ∪ （B - A∩B）
#使用左外的 (A - A∩B) union 右外的（B - A∩B）
select 字段列表
from A表 left join B表
on 关联条件
where 从表关联字段 is null and 等其他子句
union
select 字段列表
from A表 right join B表
on 关联条件
where 从表关联字段 is null and 等其他子句
```

### SQL99语法新特性

#### 自然连接

**SQL99 在 SQL92 的基础上提供了一些特殊语法，比如 {% span red,"NATURAL JOIN"%} 用来表示自然连接。我们可以把 自然连接理解为 SQL92 中的等值连接。它会帮你自动查询两张连接表中{% span red,"所有相同的字段"%} ，然后进行 {% span red,等值连接 %} 。**

* **在SQL92标准中：**

```sql
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`
AND e.`manager_id` = d.`manager_id`;
```

* **在SQL99中可以写成：**

```sql
SELECT employee_id,last_name,department_name
FROM employees e NATURAL JOIN departments d;
```

#### USING连接

**当我们进行连接的时候，SQL99还支持使用USING指定数据表里的{% span red,"同名字段" %}进行等值连接。但是只能配合JOIN一起使用。比如：**

```sql
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
USING (department_id);
```

**你能看出与自然连接 NATURAL JOIN 不同的是，USING 指定了具体的相同的字段名称，你需要在 USING 的括号 () 中填入要指定的同名字段。同时使用 {% span red,"JOIN...USING" %} 可以简化 JOIN ON 的等值连接。它与下 面的 SQL 查询结果是相同的：**

```sql
SELECT employee_id,last_name,department_name
FROM employees e ,departments d
WHERE e.department_id = d.department_id;
```

### 章节小结

**表连接的约束条件可以有三种方式:WHERE, ON, USING**

* **WHERE：适用于所有关联查询**
* **ON:只能和JOIN一起使用，只能写关联条件。虽然关联条件可以并到WHERE中和其他条件一起 写，但分开写可读性更好。**
* **USING:只能和JOIN一起使用，而且要求两个关联字段在关联表中名称一致，而且只能表示关联字 段值相等**

```sql
#关联条件
#把关联条件写在where后面
SELECT last_name,department_name
FROM employees,departments
WHERE employees.department_id = departments.department_id;
#把关联条件写在on后面，只能和JOIN一起使用
SELECT last_name,department_name
FROM employees INNER JOIN departments
ON employees.department_id = departments.department_id;
SELECT last_name,department_name
FROM employees CROSS JOIN departments
ON employees.department_id = departments.department_id;
SELECT last_name,department_name
FROM employees JOIN departments
ON employees.department_id = departments.department_id;
#把关联字段写在using()中，只能和JOIN一起使用
#而且两个表中的关联字段必须名称相同，而且只能表示=
#查询员工姓名与基本工资
SELECT last_name,job_title
FROM employees INNER JOIN jobs USING(job_id);
#n张表关联，需要n-1个关联条件
#查询员工姓名，基本工资，部门名称
SELECT last_name,job_title,department_name FROM employees,departments,jobs
WHERE employees.department_id = departments.department_id
AND employees.job_id = jobs.job_id;
SELECT last_name,job_title,department_name
FROM employees INNER JOIN departments INNER JOIN jobs
ON employees.department_id = departments.department_id
AND employees.job_id = jobs.job_id;
```

**{% span red,注意：%}**

**我们要 {% span yellow,控制连接表的数量 %}。多表连接就相当于嵌套 for 循环一样，非常消耗资源，会让 SQL 查询性能下 降得很严重，因此不要连接不必要的表。在许多 DBMS 中，也都会有最大连接表的限制。**

{% tip info %}

**【强制】超过三个表禁止 join。需要 join 的字段，数据类型保持绝对一致；多表关联查询时， 保 证被关联的字段需要有索引。**

 **说明：即使双表 join 也要注意表索引、SQL 性能。** 

**来源：阿里巴巴《Java开发手册》**{% endtip %}
