---
title: 运算符
date: 2022-10-16 19:34:24
tags: MySQL
categories: MySQL

---

## 算数运算符

| **运算符** | **名称**               | **作用**                     | **示例**         |
| ---------- | ---------------------- | ---------------------------- | ---------------- |
| **+**      | **加法运算符**         | **计算两个值或表达式的和**   | **select a + b** |
| **-**      | **减法运算符**         | **计算两个值或表达式的差**   | **select a - b** |
| *****      | **乘法运算符**         | **计算两个值或表达式的乘积** | **select a * b** |
| **/或DIV** | **除法运算符**         | **计算俩个值或表达式的商**   | **select a / b** |
| **%或MOD** | **求模（求余）运算符** | **计算两个值或表达式的余数** | **select a % b** |

### 加法与减法运算符

```sql
mysql> select 100 + 1,1 + 0.1,0.1 + 0.1 from dual;
+---------+---------+-----------+
| 100 + 1 | 1 + 0.1 | 0.1 + 0.1 |
+---------+---------+-----------+
|     101 |     1.1 |       0.2 |
+---------+---------+-----------+
1 row in set (0.00 sec)
```

**结论：**

* **只要有浮点数参与的运算结果为浮点数**
* **在Java中，+ 的左右两边如果有字符串，那么表示字符串的拼接。但是在MySQL中 + 只表示数值相加。如果遇到非数值类型，先尝试转成数值，如果转失败，就按0计算。（补充：MySQL中字符串拼接要使用字符串函数CONCAT()实现）**

### 乘法与除法运算符

```sql
mysql> select 100 * 1,100 * 1.0,100 / 1.0,100 div 1 from dual;
+---------+-----------+-----------+-----------+
| 100 * 1 | 100 * 1.0 | 100 / 1.0 | 100 div 1 |
+---------+-----------+-----------+-----------+
|     100 |     100.0 |  100.0000 |       100 |
+---------+-----------+-----------+-----------+
1 row in set (0.00 sec)
```

结论：

* **一个数除以整数后，不管是否能除尽，结果都为一个浮点数，并保留到小数点后4位；**
* **在数学运算中，0不能用作除数，在MySQL中，一个数除以0为NULL。**

### 求模（求余）运算符

```sql
mysql> select 12 % 3,12 mod 5 from dual;
+--------+----------+
| 12 % 3 | 12 mod 5 |
+--------+----------+
|      0 |        2 |
+--------+----------+
1 row in set (0.00 sec)
```

```sql
mysql> #筛选出employee_id是偶数的员工
mysql> select * from employees
    -> where employee_id mod 2 = 0;
```

## 比较运算符

**比较运算符用来对表达式左边的操作数和右边的操作数进行比较，比较的结果为真则返回1，比较的结果 为假则返回0，其他情况则返回NULL。**

**=   <=>   <>   !=   <   <=   >   >=**

### = 的使用

**字符串存在隐式转换。如果转换数值不成功，则看做0**

```sql
mysql> SELECT 1 = 2,1 != 2,1 = '1',1 = 'a',0 = 'a' 
    -> FROM DUAL;
+-------+--------+---------+---------+---------+
| 1 = 2 | 1 != 2 | 1 = '1' | 1 = 'a' | 0 = 'a' |
+-------+--------+---------+---------+---------+
|     0 |      1 |       1 |       0 |       1 |
+-------+--------+---------+---------+---------+
1 row in set, 2 warnings (0.00 sec)
```

 **两边都是字符串的话，则按照ANSI的比较规则进行比较。**

```sql
mysql> SELECT 'a' = 'a','ab' = 'ab','a' = 'b'
    -> FROM DUAL;
+-----------+-------------+-----------+
| 'a' = 'a' | 'ab' = 'ab' | 'a' = 'b' |
+-----------+-------------+-----------+
|         1 |           1 |         0 |
+-----------+-------------+-----------+
1 row in set (0.00 sec)
```

**只要有null参与判断，结果就为null**

```sql
mysql> SELECT 1 = NULL,NULL = NULL
    -> FROM DUAL;
+----------+-------------+
| 1 = NULL | NULL = NULL |
+----------+-------------+
|     NULL |        NULL |
+----------+-------------+
1 row in set (0.00 sec)
#例如
mysql> SELECT last_name,salary,commission_pct
    -> FROM employees
    -> #where salary = 6000;
    -> WHERE commission_pct = NULL;
Empty set (0.00 sec)
```

### <=>  安全等于（为NULL而生）

```sql
mysql> SELECT 1 <=> NULL, NULL <=> NULL
    -> FROM DUAL;
+------------+---------------+
| 1 <=> NULL | NULL <=> NULL |
+------------+---------------+
|          0 |             1 |
+------------+---------------+
1 row in set (0.00 sec)
```

```sql
#练习：查询表中commission_pct为null的数据有哪些
SELECT last_name,salary,commission_pct
FROM employees
WHERE commission_pct <=> NULL;
```

### <> 不等于运算符（相当于!=）

```sql
mysql> SELECT 3 <> 2,'4' <> NULL, '' != NULL,NULL != NULL
    -> FROM DUAL;
+--------+-------------+------------+--------------+
| 3 <> 2 | '4' <> NULL | '' != NULL | NULL != NULL |      
+--------+-------------+------------+--------------+      
|      1 |        NULL |       NULL |         NULL |      
+--------+-------------+------------+--------------+      
1 row in set (0.00 sec)
```

## 非符号类型运算符

### 空运算符 IS NULL \ IS NOT NULL \ ISNULL

#### 练习：查询表中commission_pct为null的数据有哪些

```sql
SELECT last_name,salary,commission_pct
FROM employees
WHERE commission_pct IS NULL;
#或
SELECT last_name,salary,commission_pct
FROM employees
WHERE ISNULL(commission_pct);
```

#### 练习：查询表中commission_pct不为null的数据有哪些

```sql
SELECT last_name,salary,commission_pct
FROM employees
WHERE commission_pct IS NOT NULL;
#或
SELECT last_name,salary,commission_pct
FROM employees
WHERE NOT commission_pct <=> NULL;
```

###  LEAST() \ GREATEST

#### 利用acsii码规则比较字母

```sql
mysql> SELECT LEAST('g','b','t','m'),GREATEST('g','b','t','m')
    -> FROM DUAL;
+------------------------+---------------------------+    
| LEAST('g','b','t','m') | GREATEST('g','b','t','m') |    
+------------------------+---------------------------+    
| b                      | t                         |    
+------------------------+---------------------------+    
1 row in set (0.00 sec)
```

#### 1.比较first_name与last_name字符串的字符利用acsii码从头逐次比较，2.比较name的长度

```sql
SELECT LEAST(first_name,last_name),LEAST(LENGTH(first_name),LENGTH(last_name))
FROM employees;
```

###  BETWEEN 条件下界1 AND 条件上界2  （查询条件1和条件2范围内的数据，包含边界）

#### 查询工资在6000 到 8000的员工信息

```sql
SELECT employee_id,last_name,salary
FROM employees
#或
#where salary between 6000 and 8000;#交换6000 和 8000之后，查询不到数据
WHERE salary >= 6000 && salary <= 8000;
```

#### 查询工资不在6000 到 8000的员工信息

```sql
SELECT employee_id,last_name,salary
FROM employees
WHERE salary NOT BETWEEN 6000 AND 8000;
```

### in (set)\ not in (set) 离散查询

#### 练习：查询部门为10,20,30部门的员工信息

```sql
SELECT last_name,salary,department_id
FROM employees
#where department_id = 10 or department_id = 20 or department_id = 30;
WHERE department_id IN (10,20,30);
```

#### 查询工资不是6000,7000,8000的员工信息

```sql
SELECT last_name,salary,department_id
FROM employees
WHERE salary NOT IN (6000,7000,8000);
```

### LIKE :模糊查询

#### % : 代表不确定个数的字符 （0个，1个，或多个）

```sql
#练习：查询last_name中包含字符'a'的员工信息
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%';
```

```sql
#练习：查询last_name中包含字符'a'且包含字符'e'的员工信息
#写法1：
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%' AND last_name LIKE '%e%';
#写法2：
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%e%' OR last_name LIKE '%e%a%';
```

#### _ ：代表一个不确定的字符

```sql
#练习：查询第3个字符是'a'的员工信息
SELECT last_name
FROM employees
WHERE last_name LIKE '__a%';
```

#### 需要使用转义字符: \ 

```sql
#练习：查询第2个字符是_且第3个字符是'a'的员工信息
SELECT last_name
FROM employees
WHERE last_name LIKE '_\_a%';
```

```sql
#或者  (了解)代表使用'$'符作为转义字符
SELECT last_name
FROM employees
WHERE last_name LIKE '_$_a%' ESCAPE '$';
```

### REGEXP \ RLIKE :正则表达式

####  REGEXP运算符

**（1）‘^’匹配以该字符后面的字符开头的字符串。** 

**（2）‘$’匹配以该字符前面的字符结尾的字符串。** 

**（3）‘.’匹配任何一个单字符。** 

**（4）“[...]”匹配在方括号内的任何字符。例如，“[abc]”匹配“a”或“b”或“c”。为了命名字符的范围，使用一 个‘-’。“[a-z]”匹配任何字母，而“[0-9]”匹配任何数字。** 

**（5）‘*’匹配零个或多个在它前面的字符。例如，“x*”匹配任何数量的‘x’字符，“[0-9]*”匹配任何数量的数字， 而“*”匹配任何数量的任何字符。**

```sql
mysql> SELECT 'shkstart' REGEXP '^shk', 'shkstart' REGEXP 't$', 'shkstart' REGEXP 'hk'
    -> FROM DUAL;
+--------------------------+------------------------+------------------------+
| 'shkstart' REGEXP '^shk' | 'shkstart' REGEXP 't$' | 'shkstart' REGEXP 'hk' |
+--------------------------+------------------------+------------------------+
|                        1 |                      1 |                      1 |
+--------------------------+------------------------+------------------------+
1 row in set (0.00 sec)
```

* **'^shk' 以字符串shk开头**

* **'t$' 以字符t结尾**

* **'hk' 字符串内含有hk**

```sql
mysql> SELECT 'atguigu' REGEXP 'gu.gu','atguigu' REGEXP '[ab]'
    -> FROM DUAL;
+--------------------------+-------------------------+
| 'atguigu' REGEXP 'gu.gu' | 'atguigu' REGEXP '[ab]' |
+--------------------------+-------------------------+
|                        1 |                       1 |
+--------------------------+-------------------------+
1 row in set (0.00 sec)
```

* **'gu.gu' gu与gu之间含有一个未知的字符**
* **[ab] 字符串内含有a或b**

##### 检索以'e'结尾的人名

```sql
mysql> select first_name, last_name
    -> from employees
    -> where first_name regexp 'e$'; 
```

### 逻辑运算符

**逻辑运算符主要用来判断表达式的真假，在MySQL中，逻辑运算符的返回结果为1、0或者NULL。**

| 运算符     | 作用     | 示例                             |
| ---------- | -------- | -------------------------------- |
| NOT或！    | 逻辑非   | SELECT NOT A                     |
| AND 或 &&  | 逻辑与   | SELECT A AND B或SELECT A && B    |
| OR 或 \|\| | 逻辑或   | SELECT A OR B 或 SELECT A \|\| B |
| XOR        | 逻辑异或 | SELECT A XOR B                   |

> **AND的优先级高于OR**

```sql
mysql> select last_name , department_id , salary
    -> from employees
    -> where department_id in (10,20) xor salary > 8000;
```



### 位运算符

**位运算符是在二进制数上进行计算的运算符。位运算符会先将操作数变成二进制数，然后进行位运算， 最后将计算结果从二进制变回十进制数。**

| 运算符 | 作用            | 示例          |
| ------ | --------------- | ------------- |
| &      | 按位与（位AND） | SELECT A & B  |
| \|     | 按位或（位OR)   | SELECT A \| B |
| ^      | 按位异或(位XOR) | SELECT  A ^ B |
| ~      | 按位取反        | SELECT ~ A    |
| >>     | 按位右移        | SELECT A >> 2 |
| <<     | 按位左移        | SELECT B << 2 |

> **在一定范围内满足：每向左移动1位，相当于乘以2；每向右移动一位，相当于除以2。**

```sql
mysql> select 1 ^ 2;
+-------+
| 1 ^ 2 |
+-------+
|     3 |
+-------+
1 row in set (0.00 sec)
```

### 运算符的优先级

| 优先级 | 运算符                                                       |
| ------ | ------------------------------------------------------------ |
| 1      | :=，=（赋值）                                                |
| 2      | l[，OR，XOR                                                  |
| 3      | &&，AND                                                      |
| 4      | NOT                                                          |
| 5      | BETWEEN，CASE，WHEN，THEN 和ELSE                             |
| 6      | =(比较运算符)，<=>，>=，>，<=，<，，!=，IS，LIKE，REGEXP和IN |
| 7      | \|                                                           |
| 8      | &                                                            |
| 9      | <<与>>                                                       |
| 10     | -和+                                                         |
| 11     | *，l，DIV，%和MOD                                            |
| 12     | ^                                                            |
| 13     | -(负号）和~(按位取反）                                       |
| 14     | !                                                            |
| 15     | ()                                                           |

> **数字编号越大，优先级越高，优先级高的运算符先进行计算。**

### 扩展：使用正则表达式查询

正则表达式通常被用来检索或替换那些符合某个模式的文本内容，根据指定的匹配模式匹配文本中符合 要求的特殊字符串。例如，从一个文本文件中提取电话号码，查找一篇文章中重复的单词或者替换用户 输入的某些敏感词语等，这些地方都可以使用正则表达式。正则表达式强大而且灵活，可以应用于非常 复杂的查询。

MySQL中使用REGEXP关键字指定正则表达式的字符匹配模式。下表列出了REGEXP操作符中常用字符匹配 列表。

| 选项        | 说明                                                         | 例子                                        | 匹配值示例              |
| ----------- | ------------------------------------------------------------ | ------------------------------------------- | ----------------------- |
| ^           | 匹配文本的开始字符                                           | '^b'匹配以字母b开头的字符串                 | book,big,banana,bike    |
| $           | 匹配文本的结束字符                                           | 'st$'匹配以st结尾的字符串                   | test,resist,persist     |
| .           | 匹配任何单个字符                                             | 'b.t'匹配任何b和t之间有一个字符的字符串     | bit,bat,but,bite        |
| *           | 匹配零个或多个在它前面的字符                                 | 'f*n'匹配字符n前面有任意个字符f的字符串     | fn,fan,faan,fabcn       |
| +           | 匹配前面的字符1次或多次                                      | 'ba+'匹配以b开头后面紧跟至少有一个a的字符串 | ba,bay,bare,battle      |
| <字符串>    | 匹配包含指定的字符串的文本                                   | '\<fa>'匹配包含fa的字符串                   | fan,afa,faad            |
| [字符集合]  | 匹配字符集合中的任何一个字符                                 | '[xz]'匹配包含x或z的字符串                  | dizzy,zebra,x-ray,extra |
| [^字符集合] | 匹配不在括号中的任何字符                                     | '\[^abc]'匹配任何不包含a、b或c的字符串      | desk,fox,f8ke           |
| 字符串{n}   | 匹配前面的字符串至少n次                                      | 'b{2}'匹配2个或更多的b                      | bbb，bbbb，bbbbbb       |
| 字符串{n,m} | 匹配前面的字符串至少n次，最多m次。如果n为0，此参数为可选参数 | b{2,4}匹配最少2个、最多4个b的字符串         | bb，bbb，bbbb           |

