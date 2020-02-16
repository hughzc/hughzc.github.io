---
title: MySQL
date: 2020-02-09 10:37:14
tags: MySQL
categories: MySQL
---

> 尚硅谷MySQL课程笔记

# 数据库操作基础

## 数据库概述

### 优点

- **可将数据持久化到硬盘**
- **可存储大量数据**
- **方便检索**
- 保证数据的一致性，完整性
- 安全，可共享
- 通过组合分析，可以产生新数据

<!-- more -->

### 常见数据库产品

- Oracle：甲骨文（产品免费，服务收费）
- DB2：IBM（兼容性相对不好）
- SQL Server：微软（兼容性不好）
- MySQL：甲骨文（开源，免费，性能高）

### 数据库相关概念

- DB

  数据库（database）：存储数据的“仓库”，保存了一系列有组织的数据

- DBMS

  数据库管理系统（Database Management System）：数据库是通过DBMS创建和操作的容器

- SQL

  结构化查询语言（Structure Query Language）：专门用来与数据库软件通信的语言

### 数据库存储数据特点

1. 将数据放到**表**中，表再放到库中

2. 一个数据库可以有多个表，每个表有唯一性的用于标识的表名

3. 表的**特性**定义了数据在表中如何存储，类似java中“类”的设计

4. 表由**列**组成，也称为字段。所有表都是由一个或多个列组成，每一列类似java中的“**属性**”

5. 表中的数据按**行**存储，每一行类似java中的“**对象**”
6. 表中的所有记录，类似于“**对象的集合**”

orm设计思想：object relation mapping 对象关系映射

## 初识MySQL

### MySQL数据库介绍

​	MySQL前身为瑞典公司AB，2008年被sun公司收购，2009sun被oracle收购。

特点：

1. 体积小，安装较方便
2. 开源，免费
3. 性能高，稳定性好
4. 兼容性好

​	MySQL是一种开源的关系型数据库管理系统，广泛应用在中小型网站中。

### DBMS分类

- 基于共享文件系统的DBMS（Access）

- 基于客户机--服务器的DBMS   C/S  （MySQL，Oracle，SqlServer）

  主要安装服务端。

### MySQL服务的启动与停止

**方式一：图形化**

右击--计算机管理--服务--MySQL服务

**方式二：通过管理员身份运行dos**

net start 服务名

net stop 服务名

### MySQL服务的登录与退出

**方式一：通过dos命令**

mysql -h主机名 -P端口号 -u用户名 -p密码

注意：

​	如果是本机，则-h主机名可以省略

​	如果端口号是3306，则-P端口号可以省略

​	默认端口号3306，要先开启服务端，以管理员身份开cmd输入net start mysql5.5，后面为自定义的mysql服务名，然后开启mysql服务，普通用户打开cmd，输入mysql -u用户名 -p密码，即可。

使用exit退出，也可以mysql -uroot -p，然后再输入密码，这样可以隐藏密码。

连接其他主机上的数据库

~~~
mysql -h主机名 -P端口号 -u用户名 -p密码
~~~

**方式二：通过图形化界面客户端**

通过sqlyog，直接输入用户名，密码等

## MySQL的常见命令和语法规范

### 常见命令

- show databases; 看数据库有哪些

  - information_schema 服务端基本信息数据

  - mysql 用户信息，表信息等

  - performance_schema 性能分析

    **前三个都不要改动！！！**

  - test

    默认为空

- show tables;                            显示当前库中所有表

- show tables from  库名;        直接看某个数据库的表 不影响当前库，因为**没有使用use切换库**。

- show columns from 表名;     显示指定表中所有列

- desc 表名;                                看表结构  describe

- use 库名;                                  使用/打开指定数据库

- select database();查看当前所在库

### 语法规范

- 不区分大小写

- 每条命令结尾建议用分号，也可以用\g

- 注释

  - 单行注释：#注释文字

  - 单行注释：-- 注释文字，中间要有空格

  - 多行注释：/* 注释文字 */

- 每条命令如果根据需要，可以进行缩进或换行

  关键字最好单独一行

## DQL语言学习 

### 分类

- DDL（Data Definition Language）：数据定义语言，用来定义数据库对象：库、表、列等；

  create/drop/alter

- **DML**（Data Manipulation Language）：数据操作语言，用来定义数据库记录（数据）；

  **insert/update/delete**

- DCL（Data Control Language）：数据控制语言，用来定义访问权限和安全级别；

  TCL(Transaction Control Language)

- **DQL**（Data Query Language）：数据查询语言，用来查询记录（数据）。

  **select**

### 基础查询

语法：

select 查询列表 from 表名;

特点：

1. 查询的结果集是一个虚拟表

2. select 查询列表 类似于System.out.println(打印内容);

   select后面跟的查询列表，可以有多个部分组成，中间用逗号隔开

   例如：select 字段1,字段2, 表达式 from 表;

   System.out.println()的打印内容，只能有一个。

3. 执行顺序

   select first_name from employees;

   先执行from语句，查询是否有此表，然后执行select语句，输出查询的列

4. 查询列表可以是：字段，表达式，常量，函数等，也可以是这些的组合

#### 查询常量

~~~sql
SELECT 100;
~~~

#### 查询表达式

~~~sql
SELECT 100/3;
~~~

#### 查询单个字段

~~~sql
SELECT `last_name` FROM `employees`;
SELECT last_name FROM employees;
~~~

​	注意，着重号的使用是为了区分关键字，如某一column名为name，但同时NAME为关键字，这时候需要加着重号。如果不为关键字，则不用加。为了快捷输入，可以双击名称。

{% asset_img 查询单个字段.png This is an example image %}

#### 查询多个字段

~~~sql
SELECT `first_name`,`last_name`,`email` FROM `employees`;
~~~

​	中间用逗号隔开。

{% asset_img 查询多个字段.png This is an example image %}

#### 查询所有字段

~~~sql
SELECT * FROM `employees`;
~~~

{% asset_img 查询所有字段.png This is an example image %}

​	不好点在于字段顺序为表中定义顺序，因此开发中更常用的是将所有字段按照需要的顺序点击，如果太长不方便阅读，使用快捷键F12，来自动换行。

#### 查询函数（调用函数，获取返回值）

~~~sql
SELECT DATABASE(); #查询当前的数据库
SELECT VERSION();  #查询当前数据库服务器版本
SELECT USER();     #查询当前用户
~~~

#### 起别名

方式一：使用as关键字

~~~sql
SELECT USER() AS 用户名;
SELECT USER() AS "用户名";
SELECT USER() AS '用户名';
~~~

使用前

{% asset_img 起别名前.png This is an example image %}

使用后

{% asset_img 起别名后.png This is an example image %}

​	加双引号跟单引号是为了方便识别，如下面例子

~~~sql
SELECT `last_name` AS 姓 名 FROM `employees`;    #报错，因为不知道名是干嘛的
SELECT `last_name` AS "姓 名" FROM `employees`; 
~~~

​	如果姓名中间有空格，不加双引号会报错。

方式二：使用空格

~~~sql
SELECT USER() "用户名";
~~~

#### concat的使用

-- 需求：查询 first_name和last_name拼接成的全名，最终起别名为：姓 名

方案一：使用+号

​	如果是按照Java的思路

~~~sql
SELECT first_name+last_name AS "姓 名" FROM employees;
~~~

但是出来结果为

{% asset_img 加法.png This is an example image %}

​	这是因为在Java中+可以执行加法运算，也可以实现字符串的拼接（至少要求一个为字符串）。

​	但在MySQL中，+作用为

1. 加法运算

   - 两个操作数为数值型

     100+1.5

   - 其中一个操作数为字符型

     将字符型数据强制转换成数值型，如果无法转换，则直接当做0处理

     ‘张无忌’+100 ===> 100

   - 其中一个操作数为null

     null+null ===> null

     null+100 ===> null

因此两个char直接相加，都不能识别，结果为0。

方案二：使用concat()拼接函数

~~~sql
SELECT CONCAT(first_name,last_name) AS "姓 名" 
FROM employees;
~~~

​	concat()函数中支持任意长度的String。

{% asset_img concat拼接.png This is an example image %}

#### distinct去重

需求：查询员工涉及到的部门编号有哪些

​	如有10个员工都是部门90，有10个员工都是部分80，将所有部门编号不重复的显示出来。

如果直接敲入SELECT department_id FROM employees;，那么会出现重复的编号如下。

{% asset_img 重复部门编号.png This is an example image %}

​	可以通过增加distinct关键字来去重

~~~sql
SELECT DISTINCT `department_id` FROM `employees`;
~~~

​	显示结果为

{% asset_img 编号去重.png This is an example image %}

#### 查看表的结构

~~~sql
DESC employees;
SHOW COLUMNS FROM `employees`;
~~~

​	上面两种都可以显示表所有列的结构

{% asset_img 显示表结构.png This is an example image %}

### 练习题

1、下面的语句是否可以执行成功

~~~sql
SELECT 
  last_name,
  job_id,
  salary AS sal 
FROM
  employees ;
~~~

​	可以，输出多个，将salary起名为sal。

2、下面语句是否可以执行成功

~~~sql
SELECT * FROM employees ;
~~~

​	可以，输出eployees中全部。

3、找出下面语句中的错误

~~~sql
SELECT 
  employee_id,
  last_name，
  salary * 12 “ ANNUAL SALARY ” 
FROM
  employees ;
~~~

​	错误在于last_name后面的逗号为中文逗号，12后面的双引号为中文的符号，更改即可。salary*12起名为ANNUAL SALARY。

4、显示表 departments 的结构，并查询其中的全部数据 

~~~sql
 DESC departments;
 SELECT * FROM departments;
~~~

​	可以使用desc，也可以使用SHOW COLUMNS FROM departments;

5、显示出表 employees 中的全部 job_id （不能重复）

~~~sql
 SELECT DISTINCT job_id FROM employees;
~~~

​	考distinct关键字的使用，查哪个表，就from哪个。

6、显示出表 employees 的全部列，各个列之间用逗号连接，列头显示成 OUT_PUT 

~~~sql
 SELECT CONCAT(employee_id,',',first_name,',',last_name,',',commission_pct) AS "OUT_PUT"
 FROM employees;
~~~

​	考concat使用，在不同列之间还要拼接上‘,’，然后起名。

​	但是有的为null，有的是正常的。

{% asset_img 带逗号拼接.png This is an example image %}

​	然后调用SELECT commission_pct FROM employees;，发现有的commission_pct为null值。在拼接时，只要有一个为null，那么就不能直接拼接。这时候需要判断，只有在不为空的时候才进行拼接。使用函数ifnull。

~~~sql
SELECT commission_pct, IFNULL(commission_pct,"空") FROM employees;
~~~

​	此函数需要输入两个参数，一个是进行判断的参数，第二个是如果此参数为null要显示的参数。

{% asset_img IFNULL使用.png This is an example image %}

​	因此将commission进行替换为ifnull即可

~~~sql
SELECT CONCAT(employee_id,',',first_name,',',last_name,',',IFNULL(commission_pct,'')) AS "OUT_PUT" FROM employees;
~~~

### 条件查询

#### 语法：

select 查询列表 

from 表名

**where 筛选条件**;

​	执行顺序：1，from；2，where；3，select。

​	在没有索引时逐行查询，再筛选有where满足的语句，进行select输出。

~~~sql
SELECT `first_name`,`last_name` FROM `employees` WHERE `salary`>2000;
~~~

#### 特点：

1. 按关系表达式筛选

   关系运算符：

   | 小于     | <    |
   | -------- | ---- |
   | 小于等于 | <=   |
   | 大于     | >    |
   | 大于等于 | >=   |
   | 等于     | =    |
   | 不等于   | <>   |

   补充：不等于!=也可以，但不建议

2. 按逻辑表达式

   逻辑运算符

   | 与   | and  |
   | ---- | ---- |
   | 或   | or   |
   | 非   | not  |

   补充：也可以使用&&   ||    !   但不建议

3. 模糊查询
   - like
   - in
   - between and
   - is null

#### 按关系表达式查询

案例1：查询部门编号不是100的员工信息

~~~sql
 SELECT *
 FROM employees
 WHERE department_id <> 100;
~~~

​	先按照select,from,where写好，然后进行填充。输出的是员工信息，但是不确定，用*号，然后选择的表是员工表，条件是部门编号不为100。的后跟什么，select后加什么。的前面是什么，where后加什么

案例2：查询工资小于15000的姓名、工资

~~~sql
 SELECT last_name,salary
 FROM employees
 WHERE salary<15000;
~~~

​	的后为姓名工资，放在select后，的前为工资小于15000，放在where后。

#### 按逻辑表达式查询

案例1：查询部门编号不是50-100之间员工姓名、部门编号、邮箱

方式1：

~~~sql
SELECT last_name,department_id,email
FROM employees
WHERE department_id > 100 OR department_id < 50;
~~~

方式2：

~~~sql
SELECT last_name,department_id,email
FROM employees
WHERE NOT(department_id >= 50 AND department_id <= 100);
~~~

案例2：查询奖金率>0.03 或者员工编号在60-110之间的员工信息

~~~sql
SELECT *
FROM employees
WHERE commission_pct > 0.03 OR (employee_id >= 60 AND employee_id <= 110);
~~~

#### 模糊查询

##### like

功能：一般和通配符搭配使用，对字符型数据进行**部分匹配查询**

like/ not like

常见的通配符：

- _    任意单个字符
- %   任意多个字符，支持0到多个字符

案例1：查询姓名中包含字符a的员工信息

~~~sql
SELECT *
FROM employees
WHERE last_name LIKE '%a%';
~~~

​	姓名中包含a，使用like，因为a前面有几个字符不知道，因此a前面加%；a后面有几个字符不知道，因此a后面加%。而sql中字符要用单引号括起来，因此是like ‘%a%’。

案例2：查询姓名中最后一个字符为e的员工信息

~~~sql
SELECT *
FROM employees
WHERE last_name LIKE '%e';
~~~

​	最后个字符为e，因此e前面有几个字符不知道，但e后面没有字符，因此是like ‘%e’。

案例3：查询姓名中第一个字符为e的员工信息

~~~sql
SELECT *
FROM employees
WHERE last_name LIKE 'e%';
~~~

​	第一个个字符为e，因此e后面有几个字符不知道，但e前面没有字符，因此是like ‘e%’。

案例4：查询姓名中第三个字符为x的员工信息

~~~sql
SELECT *
FROM employees
WHERE last_name LIKE '__x%';
~~~

​	x前面有2个字符，因此添加2个下划线_，然后x后面的字符个数位置，使用%，因此为‘__x%’。

案例5：查询姓名中包含第二个字符为_的员工信息

~~~sql
SELECT *
FROM employees
WHERE last_name LIKE '_\_%';
~~~

​	第二个下划线需要使用转义字符在转义。

​	但这种在sql中不是常见语法

~~~sql
SELECT *
FROM employees
WHERE last_name LIKE '_$_%' ESCAPE '$';
~~~

​	使用一个喜欢的而且不常见的字符作为转义字符，利用ESCAPE来标记其为转义字符。

##### in

功能：查询某字段的值是否**属于指定的列表**之内

a  in(常量值1,常量值2,常量值3,….)  意思是看a是否是在这些值之内

a not in(常量值1,常量值2,常量值3,….)  意思是看a是否是均在这些值之内

in/not in  在或者不在

案例1：查询部门编号是30/50/90的员工名和部门编号

方式1：

~~~sql
SELECT last_name,department_id
FROM employees
WHERE department_id IN(30,50,90);
~~~

方式2：

~~~sql
SELECT last_name,department_id
FROM employees
WHERE department_id =30
OR department_id =50
OR department_id =90;
~~~

案例2：查询工种编号不是SH_CLERK或IT_PROG的员工信息

方式1：

~~~sql
SELECT *
FROM employees
WHERE job_id NOT IN('SH_CLERK','IT_PROG');
~~~

方式2：

~~~sql
SELECT *
FROM employees
WHERE NOT (job_id = 'SH_CLERK'
OR job_id = 'IT_PROG');
~~~

##### between and

功能：判断某个字段的值是否介于xx之间。判断的是区间值。**包含临界值**。

between and / not between and

案例1：查询部门编号是30-90之间的部门编号、员工姓名

方式1：

~~~sql
SELECT department_id,last_name
FROM employees
WHERE department_id BETWEEN 30 AND 90;
~~~

​	between和and之间，要**先写小的数，再写大的**。

方式2：

~~~sql
SELECT department_id,last_name
FROM employees
WHERE department_id >= 30 AND department_id <= 90;
~~~

​	两种方式是等价的，因此一定要先写小的，不然查询不到。

案例2：查询年薪不是100000-200000之间的员工姓名、工资、年薪

~~~sql
SELECT last_name,salary,salary*12*(1+IFNULL(commission_pct,0)) 年薪
FROM employees
WHERE salary*12*(1+IFNULL(commission_pct,0)) NOT BETWEEN 100000 AND 200000;
~~~

​	首先在表格中没有年薪，因此要明确年薪的计算

> 年薪=月薪x12x(1+奖金率)

​	但奖金率有可能为空，因此要使用ifnull进行判断，如果为空则设置为0，然后给年薪起名，条件语句是年薪不在100000和200000之间，因此使用not between。

​	小技巧：可以先进行select和from进行判断语句是否写正确，然后再写where语句。

##### is null

is null / is not null

案例1：查询没有奖金的员工信息

~~~sql
SELECT *
FROM employees
WHERE commission_pct IS NULL;
~~~

​	直接用=null无法判断出来。

案例2：查询有奖金的员工信息

~~~sql
SELECT *
FROM employees
WHERE commission_pct IS NOT NULL;
~~~

=与is null比较

- =               只能判断普通的内容
- IS              只能判断NULL值
- <=>          安全等于，既能判断普通内容，又能判断NULL值，但**可读性差**

### 排序查询

语法：

select 查询列表

from 表格

where 筛选条件  （**可省略**）

order by 排序列表

执行顺序

1. from 子句

2. where 子句

3. select 子句

4. order by 子句

   **先查出来，再排序**

特点：

1. 排序列表可以是单个字段、多个字段、表达式、函数、列数以及以上的组合

2. 升序，通过  asc ， 默认行为

   降序，通过 desc

#### 按单个字段排序

案例1：将员工编号大于120的员工信息进行工资的升序

~~~sql
SELECT *
FROM employees
WHERE employee_id > 120
ORDER BY salary ASC;
~~~

​	其中ASC可以省略。

案例2：将员工编号大于120的员工信息进行工资的降序

~~~sql
SELECT *
FROM employees
WHERE employee_id > 120
ORDER BY salary DESC;
~~~

#### 按表达式排序

案例1：对有奖金的员工，按年薪降序

~~~sql
SELECT *,salary*12*(1+IFNULL(commission_pct,0))  年薪
FROM employees
WHERE commission_pct IS NOT NULL
ORDER BY salary*12*(1+IFNULL(commission_pct,0)) DESC;
~~~

​	计算出年薪表达式，然后选择奖金率不为空，再按照年薪排序，但表达式太长。

#### 按别名排序

案例1：对有奖金的员工，按年薪降序

~~~sql
SELECT *,salary*12*(1+IFNULL(commission_pct,0))  年薪
FROM employees
WHERE commission_pct IS NOT NULL
ORDER BY 年薪 DESC;
~~~

​	计算出年薪表达式，然后选择奖金率不为空，再按照年薪排序，但表达式太长。

​	判断：where判断处是否能使用年薪？

​	不能，因为执行顺序是from,where,select,order by，在where处别名年薪还没有被定义，因此只能在order by后使用。

​	这里语法可以被简化，因为经过where判断后，已经不存在奖金率为null的情况，因此不用在select中加上对奖金率IFNULL的判断。

~~~sql
SELECT *,salary*12*(1+commission_pct)  年薪
FROM employees
WHERE commission_pct IS NOT NULL
ORDER BY 年薪 DESC;
~~~

#### 按函数的结果排序

案例1：按姓名的字数长度进行升序

获取字数长度可以使用LENGTH()函数。

~~~sql
SELECT LENGTH(last_name),last_name
FROM employees;
~~~

{% asset_img LENGTH函数.png This is an example image %}

​	因此直接order by length(last_name)即可

~~~sql
SELECT last_name
FROM employees
ORDER BY LENGTH(last_name);
~~~

#### 按多个字段排序

案例1：查询员工的姓名、工资、部门编号，先按工资升序，再按部门编号降序

~~~sql
SELECT last_name,salary,department_id
FROM employees
ORDER BY salary ASC, department_id DESC;
~~~

​	总体先 按照工资排序，当有工资相同的情况，按照部门编号降序

{% asset_img 工资升序编号降序.png This is an example image %}

​	中间用逗号隔开，每个排序方式都是相独立指定的。

#### 按列数排序，用的少

案例1：查询员工信息，按照第二列排序

~~~sql
SELECT * FROM employees
ORDER BY 2;
~~~

​	用的较少，阅读性较差



## 基本函数

### select database()

​	获取当前使用的数据库

### select user()

​	获取当前用户信息

### select version()

​	获取版本号

### concat()

​	concat(str1,str2,…,strn)

​	将n个字符进行拼接，如果其中一个为null，则所有均为null。

### ifnull()

​	ifnull(表达式1，表达式2)

表达式1：可能为null的字段或表达式；表达式2：如果表达式1为null，则最终结果显示的值

功能：如果表达式1为null，则显示表达式2；否则显示表达式1。

### length()

​	length(字段)

​	获取字段的长度





# 快捷键

## MySQL客户端

- F12 将选中语句自动换行