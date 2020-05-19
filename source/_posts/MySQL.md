---
b title: MySQL
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

### 数据库存储

局部性原理：程序和数据的访问有聚集成群的倾向，在一个时间段内，仅使用其中一小部分（空间局部性），或最近访问的程序代码和数据，很快又被访问的可能性很大（时间局部性）。

- 默认数据在硬盘中存储以页为逻辑单元，在InnoDB中，一页为16kb

  在磁盘中，页是存储器的逻辑块，每个存储块为一页（许多操作系统中，页大小通常为4k），主存和磁盘以页为大小交换数据

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

- select database();                   查看当前所在库

### 语法规范

- 不区分大小写

- 每条命令结尾建议用分号，也可以用\g

- 注释

  - 单行注释：#注释文字

  - 单行注释：-- 注释文字，中间要有空格

  - 多行注释：/* 注释文字 */

- 每条命令如果根据需要，可以进行缩进或换行

  关键字最好单独一行

## DQL语言 

### 分类

- DDL（Data Definition Language）：数据定义语言，用来定义数据库对象：库、表、列等；

  create/drop/alter

- **DML**（Data Manipulation Language）：数据操作语言，用来定义数据库记录（数据）；

  **insert/update/delete**

- DCL（Data Control Language）：数据控制语言，用来定义访问权限和安全级别；

  TCL(Transaction Control Language)

- **DQL**（Data Query Language）：数据查询语言，用来查询记录（数据）。

  **select**

### 进阶一：基础查询

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
SELECT USER() AS `用户名`;
~~~

使用前

{% asset_img 起别名前.png This is an example image %}

使用后

{% asset_img 起别名后.png This is an example image %}

​	加**着重号**是为了方便识别，如下面例子。注意：是着重号而不是引号

~~~sql
SELECT last_name AS 姓 名 FROM `employees`;    #报错，因为不知道名是干嘛的
SELECT last_name AS "姓 名" FROM `employees`; 
~~~

​	如果姓名中间有空格，不加**着重号**会报错。

方式二：使用空格

~~~sql
SELECT USER() `用户名`;
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

### 练习题1

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

### 进阶二：条件查询

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

### 进阶三：排序查询

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

3. order by的位置一般放在查询语句的最后（除limit语句之外）

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

### 练习题2

1、查询员工的姓名和部门号和年薪，按年薪降序 按姓名升序

~~~sql
SELECT last_name,department_id,salary*12*(1+IFNULL(commission_pct,0)) 年薪
FROM employees
ORDER BY 年薪 DESC, last_name ASC;
~~~

疑问：当order by中别名加了引号后就不能正确排序，如下所示

{% asset_img 引号排序.png This is an example image %}

​	后经人提醒，应该是**着重号**而不是引号，因此正确的使用别名方式应该如下

~~~sql
SELECT last_name,department_id,salary*12*(1+IFNULL(commission_pct,0)) `年 薪`
FROM employees
ORDER BY `年 薪` DESC, last_name ASC;
~~~

2、选择工资不在8000到17000的员工的姓名和工资，按工资降序

~~~sql
SELECT last_name,salary
FROM employees
WHERE salary NOT BETWEEN 8000 AND 17000
ORDER BY salary DESC;
~~~

3、查询邮箱中包含e的员工信息，并先按邮箱的字节数降序，再按部门号升序

~~~sql
SELECT *
FROM employees
WHERE email LIKE '%e%'
ORDER BY LENGTH(email) DESC, department_id ASC;
~~~

### 进阶四：常见函数

函数：类似于java中的“方法”，为了解决某个问题，将编写的一系列的命令集合封装在一起，对外仅仅暴露方法名，供外部调用。

1、自定义方法（函数）

2、调用方法啊（函数）

- 叫什么：函数名
- 干什么：函数功能

常见函数：

- 字符函数
- 数学函数
- 日期函数
- 流程控制函数

#### 字符函数

1、CONCAT 拼接函数

~~~sql
SELECT CONCAT('hello,',first_name,last_name) FROM employees;
~~~

​	可以指定多个参数

2、LENGTH 获取**字节**长度

​	一个汉字识别为3个字节

~~~sql
SELECT LENGTH('hello,李四');
~~~

​	输出为12。

3、CHAR_LENGTH 获取**字符**长度

~~~sql
SELECT CHAR_LENGTH('hello,李四');
~~~

​	输出为8。

4、SUBSTRING 截取子串

~~~sql
SELECT SUBSTR('武汉加油，中国加油',1,4);
~~~

substr(str，起始索引，截取的字符长度)  **起始索引从1开始**！！！	

substr(str，起始索引)	默认将后面全部截取

第一个参数为完整字符串，第二个参数为起始位置，其中SQL中从1开始，第三个参数为截取长度。

5、INSTR 获取字符第一次出现的索引

~~~sql
SELECT INSTR('三打白骨精aa白骨精','白骨精');
~~~

​	结果为3，因为白骨精第一次出现的索引是3。

6、TRIM 去前后指定的字符，默认是去空格

~~~sql
SELECT TRIM('   虚 竹   ') AS `a`;
~~~

结果为虚 竹。前后空格被去掉，中间不去掉

~~~sql
SELECT TRIM('x' FROM 'xxxxx虚xx竹xxxx') AS `a`;
~~~

结果为虚xx竹，将指定的字符从字符串中去掉了，中间的不受影响。

7、LPAD/RPAD  左填充/右填充

~~~sql
SELECT LPAD('武汉加油',10,'a');
~~~

结果为aaaaaa武汉加油，往左边填充指定的字符，至指定长度。

也可以用来**截取字符串**

~~~sql
SELECT LPAD('庆历四年滕子京谪守巴陵郡',4,'a');
~~~

当指定长度比输入字符串长度更短时，会截取字符串为指定长度，因此输出为庆历四年。

~~~sql
SELECT RPAD('武汉加油',10,'a');
~~~

​	输出结果为武汉加油aaaaaa，在右边填充。

~~~sql
SELECT RPAD('庆历四年滕子京谪守巴陵郡',4,'a');
~~~

​	结果同样为庆历四年。

8、UPPER/LOWER  变大写/小写

案例：查询员工表中的姓名，要求格式：姓首字符大写，其他字符小写，名所有字符大写，且姓和名之间用_分割，最后起别名“OUTPUT”。

~~~sql
SELECT CONCAT(SUBSTR(first_name,1,1),LOWER(SUBSTR(first_name,2)),'_',UPPER(last_name)) AS `OUTPUT`
FROM employees;
~~~

9、STRCMP 比较两个字符大小

STRCMP(str1,str2)

​	如果str1比较大，输出1；如果str2比较大，输出-1；如果两个一样大，返回0。

​	**字符均用单引号括起来**。

​	对应String compareTo()方法，其从首位比较两个字符串，如果第一个不等，则返回二者ASCII码差值，如果相等，再比较第二个，以此类推。如果比较到最后都一样（较短长度的字符串位置），则返回二者长度的差值。

10、LEFT/RIGHT  截取子串

​	从左边截取指定的长度或从右边截取指定的长度

~~~sql
SELECT LEFT('武汉加油',1);
SELECT RIGHT('武汉加油',2);
~~~

分别输出武和加油。

11、REPLACE 替换

REPLACE（str, a, b）	用字符串b替换字符串str中所有出现的字符串a

#### 数学函数

1、ABS 绝对值

2、CEIL 向上取整，返回**>=**该参数的最小整数

~~~sql
SELECT CEIL(1.1);
SELECT CEIL(1.0);
SELECT CEIL(-1.09);
~~~

​	结果为2和1和-1，因为是大于等于。

3、FLOOR 向下取整，返回<=该参数的最大整数

~~~sql
SELECT FLOOR(-1.09);
SELECT FLOOR(0.09);
SELECT FLOOR(1.00);
~~~

​	结果为-2,0和1，因为是小于等于。

4、ROUND 四舍五入

~~~sql
SELECT ROUND(1.87);
~~~

​	结果为2，小数点后没有保存位数。

​	但是想保留两个小数，可以增加一个参数。

~~~sql
SELECT ROUND(1.8712345,2);
~~~

​	结果为1.87。

5、**TRUNCATE** 截断

truncate(num,count)

​	在数字小数点后保留指定位数。

~~~sql
SELECT TRUNCATE(1.8723131,1);
~~~

​	小数点后保留一位小数，结果为1.8。

6、MOD 取余

**被除数的正负决定了结果的正负**

​	与%结果一样。

~~~sql
SELECT MOD(-10,3);
SELECT MOD(10,-3);
~~~

​	结果为-1和1，正负决定于被除数，即前面一个数。

 A%B = A - (A/B)*B

​	小技巧：绝对值取余再看正负。

6、RAND 随机数

RAND(x)	返回0~1的随机值

7、SQRT 平方根

SQRT(x)	返回x的平方根

8、POW 次方

POW(x,y)	返回x的y次方

#### 日期函数

1、NOW  获取当前时间+时间，包括日期、小时

~~~sql
SELECT NOW();
~~~

{% asset_img NOW函数.png This is an example image %}

2、CURDATE  获取当前日期

~~~sql
SELECT CURDATE();
~~~

{% asset_img 当前日期.png This is an example image %}

3、CURTIME 获取当前时间，只有小时

~~~sql
SELECT CURTIME();
~~~

{% asset_img 当前时间.png This is an example image %}

4、DATEDIFF 获取两个日期之差

~~~sql
SELECT DATEDIFF('2020-2-17','2018-6-24');
~~~

​	输出两个日期时间，前面日期减去后面，得到天数。

5、DATE_FORMAT 格式转换

~~~sql
SELECT DATE_FORMAT('2020-2-17','%Y年%m月%d日 %H小时%i分钟%s秒') `当前日期`;
~~~

年月日小时分秒有规定的格式

{% asset_img 日期转换.png This is an example image %}

​	如可以查看员工的入职日期

~~~sql
SELECT DATE_FORMAT(hiredate,'%Y年%m月%d日 %H小时%i分钟%s秒') `入职日期`
FROM employees;
~~~

6、STR_TO_DATE 按指定格式解析字符串为日期

查看入职日期比指定日期小的人

~~~sql
SELECT *
FROM employees
WHERE hiredate < STR_TO_DATE('3/15 1998','%m/%d %Y');
~~~

这样就可以进行比较。

7、返回具体的时间值

YEAR(date)
MONTH(date)
DAY(date)
HOUR(time)
MINUTE(time)
SECOND(time)	

#### 其他函数

version 当前数据库服务器的版本

database 当前打开的数据库

user 当前用户

password(‘字符’) 返回该字符的密码形式

md5(‘字符’) 返回该字符的md5加密形式

#### 流程控制函数

1、IF函数

if(条件表达式，表达式1，表达式2)

如果条件表达式成立，返回表达式1，否则返回表达式2

~~~sql
SELECT IF(100>9,'good','bad');
~~~

​	如果成立，输出第二个，不然输出第三个。类似于三目运算符。

需求：如果有奖金，则显示最终奖金，如果没有，则显示0

~~~sql
SELECT IF(commission_pct IS NOT NULL,salary*12*commission_pct,0) `奖金`
FROM employees
ORDER BY `奖金` ASC;
~~~

2、CASE函数

情况1：类似于switch语句，可以实现等值判断

CASE 表达式

WHEN 值1 THEN 结果1

WHEN 值2 THEN 结果2

…

ELSE 结果n

END

案例：部门编号是30，工资显示为2倍；部门编号是50，工资显示为3倍；部门编号是60，工资显示为4倍；否则不变。显示部门编号，新工资，旧工资

~~~sql
SELECT department_id,salary,
CASE department_id
WHEN 30 THEN salary*2
WHEN 50 THEN salary*3
WHEN 60 THEN salary*4
ELSE salary 
END `new salary`
FROM employees;
~~~

​	case开头到end结尾均为新工资，因此要起别名的话要**写在end后面**。

情况2：类似于多重IF语句，实现区间判断。

CASE

WHEN 条件1 THEN 结果1

WHEN条件2 THEN 结果2

…

ELSE 结果n

END

案例：如果工资>20000，显示级别A；如果工资>15000，显示级别B；如果工资>10000，显示级别C；否则，显示D。

~~~sql
SELECT salary,
CASE
WHEN salary>20000 THEN 'A'
WHEN salary>15000 THEN 'B'
WHEN salary>10000 THEN 'C'
ELSE 'D'
END `grade`
FROM employees;
~~~

​	WHEN后面记得要加THEN。

### 练习题3

1、显示系统时间（注：日期+时间）

~~~sql
SELECT NOW();
~~~

2、查询员工号，姓名，工资，以及工资提高百分之20%后的结果（new salary）

~~~sql
SELECT employee_id,last_name,salary,salary*1.2 `new lalary`
FROM employees;
~~~

3、将员工的姓名按首字母排序，并写成姓名的长度（length）

~~~sql
SELECT LENGTH(last_name) `长度`
FROM employees
ORDER BY SUBSTR(last_name,1,1) ASC;
~~~

或者使用LEFT函数

~~~sql
SELECT LENGTH(last_name) `长度`
FROM employees
ORDER BY LEFT(last_name,1) ASC;
~~~

4、做一个查询，产生下面的效果

<last_name> earns < salary> monthly but wants <salary*3> Dream Salary

King earns 24000 monthly but wants 72000

~~~sql
SELECT CONCAT(last_name,' earns ',salary,' monthly but wants ',salary*3) `Dream Salary`
FROM employees;
~~~

5、使用case-when，按照下面的条件：

job grade

AD_PRES A

ST_MAN B

IT_PROG C

SA_REP D

ST_CLERK E

产生下面的结果

Last_name Job_id Grade 

king AD_PRES A

~~~sql
SELECT last_name,job_id,
CASE job_id
WHEN 'AD_PRES' THEN 'A'
WHEN 'ST_MAN' THEN 'B'
WHEN 'IT_PROG' THEN 'C'
WHEN 'SA_REP' THEN 'D'
WHEN 'ST_CLERK' THEN 'E'
END `Grade`
FROM employees;
~~~

​	**凡是常量值均瑶用引号括起来**！！！

以上为单行函数，一列出来一个值，还有分组函数，多列进去出来一个值。

### 分组函数

​	分组函数往往用于实现将一组数据进行统计计算，最终得到一个值，又称为聚合函数或统计函数

1、分组函数清单

- sum(字段名) ：求和
- avg(字段名) ：求平均数
- max(字段名) ：求最大值
- min(字段名)：求最小值
- count(字段名) ：计算非空字段值的个数

2、支持的类型：

- sum和avg一般用于处理数值型
- max，min，count可以处理任何数据类型

3、以上分组函数都忽略NULL

4、都可以搭配DISTINCT使用，实现去重的统计

select max(distinct 字段) from 表

5、和分组函数一同查询的字段，要求是group by后出现的字段

案例1：查询员工信息表中，所有员工的工资和、工资平均值、最低工资、最高工资、有工资的个数

~~~sql
SELECT SUM(salary),AVG(salary),MIN(salary),MAX(salary),COUNT(salary) FROM employees;
~~~

案例2：添加筛选条件

1、查询emp表中记录数

~~~sql
SELECT COUNT(employee_id) FROM employees;
~~~

​	选择一个不可能为空的，计算其count即可

2、查询emp表中有佣金的人数

~~~sql
SELECT COUNT(salary) FROM employees;
~~~

3、查询emp表中月薪大于2500的人数

~~~sql
SELECT COUNT(salary) FROM employees
WHERE salary>2500;
~~~

​	天剑查询写在where中。

4、查询有领导的人数

COUNT本身去掉了空值，因此可以不用WHERE判断

~~~sql
SELECT COUNT(manager_id) FROM employees;
~~~

#### count的补充介绍

1、统计结果集的行数，推荐使用count(*)

查询结果中的总行数，用*统计行数较多。

~~~sql
SELECT COUNT(*) FROM employees;
SELECT COUNT(*) FROM employees WHERE department_id = 30;
~~~

这样可以不用关心count中到底要写哪个字段，可以用于方便获取行数。

也可以使用常量值

~~~sql
SELECT COUNT(1) FROM employees;
~~~

相当于加了一个常量列，但是效率与可读性没*高。

效率上：

MyISAM存储引擎，count(*)最高

InnoDB存储引擎，count(*)和count(1)效率>count(字段)

2、搭配distinct实现去重的统计

需求：查询有员工的部门个数

~~~sql
SELECT COUNT(DISTINCT department_id) FROM employees;
~~~

### 进阶五：分组查询

#### 分组查询引入

思考题：求每个部门的总工资，平均工资。需要使用分组查询。

{% asset_img 分组查询.png This is an example image %}

​	使用GROUP BY 关键字

~~~sql
SELECT SUM(salary) `总工资`,AVG(salary) `平均工资`,department_id
FROM employees
GROUP BY department_id;
~~~

出来结果如下

{% asset_img 分组查询结果.png This is an example image %}

​	查询需要一一对应，否则无意义。

#### 语法

select 查询列表

from 表名

where 筛选条件

group by 分组列表

having 分组后筛选

order by 排序列表;

执行顺序

1. from子句
2. where子句
3. group by子句
4. having子句
5. select子句
6. order by子句

#### 特点

1. 查询列表往往是   分组函数和被分组的字段

2. 分组查询中的筛选分为两类

   |            | 筛选的基表         | 使用的关键词 | 位置         |
   | ---------- | ------------------ | ------------ | ------------ |
   | 分组前筛选 | 原始表(from后的表) | where        | group by前面 |
   | 分组后筛选 | 分组后的结果集     | having       | group by后面 |

   where—group by—having

   分组函数做条件只可能放在having后面

3. group by后面可以接函数

#### 简单的分组

案例1：查询每个工种的员工平均工资

~~~sql
SELECT AVG(salary),job_id
FROM employees
GROUP BY job_id;
~~~

select后将分组函数和被分组字段放在一起。

案例2：查询每个领导的手下人数

~~~sql
SELECT COUNT(*),manager_id
FROM employees
WHERE manager_id IS NOT NULL
GROUP BY manager_id;
~~~

人数，使用COUNT(*)来查询，用领导id来分组。将领导id为空的去掉。

#### 可以实现分组前的筛选

案例1：查询邮箱中包含a字符的每个部门的最高工资

~~~sql
SELECT MAX(salary) `最高工资`,department_id
FROM employees
WHERE email LIKE '%a%'
GROUP BY department_id;
~~~

案例2：查询每个领导收下有奖金的员工的平均工资

~~~sql
SELECT AVG(salary) `平均工资`,manager_id
FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY manager_id;
~~~

按照领导分组。

#### 可以实现分组后的筛选

案例1：查询哪个部门的员工个数>5

分析1：查询每个部门的员工个数

~~~sql
SELECT COUNT(*) `员工个数`,department_id
FROM employees
GROUP BY department_id;
~~~

分析2：在刚才的结果基础上，筛选哪个部门的员工个数>5

因此思路是在where后加count(*)>5的判断，但会报错，原因在于select执行在where后，count函数此时还无意义。此时where的条件只能看到from后面emp的表，没有包含count()这一列，因此应该放在group by后面，但会报错，因为此时不应该用where，而是使用having连接词，**having才支持后面加分组函数和分组后的筛选**。

~~~sql
SELECT COUNT(*) `员工个数`,department_id
FROM employees
GROUP BY department_id
HAVING COUNT(*)>5;
~~~

小结：分组前筛选，还没有group by，此时能够筛选的列只能够从from后面的原始表格列中获取；而分组后筛选，基于的表是分组后的表，因此此时可以按照分组后的结果来筛选。为了区分分组前和分组后，使用关键词不同，分组前为where，分组后为having。

案例2：每个工种有奖金的员工的最高工资>12000的工种编号和最高工资

~~~sql
SELECT job_id,MAX(salary)
FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY job_id
HAVING MAX(salary)>12000;
~~~

有奖金的判断在分组前，最高工资的判断在分组后。

案例3：领导编号>102的每个领导手下的最低工资大于5000的领导编号和最低工资

~~~sql
SELECT manager_id,MIN(salary) `最低工资`
FROM employees
WHERE manager_id > 102
GROUP BY manager_id
HAVING MIN(salary) > 5000
~~~

将题干分段，先查询每个领导手下的最低工资，筛选刚才的结果。领导编号可以在分组前计算出来，因此可以放在where这。也可以放在having那，虽然结果一样，但这样效率比较低。因此既能放where和having的，优先放在where后面。

#### 可以实现排序

案例：查询没有奖金的员工的最高工资>6000的工种编号和最高工资，按最高工资升序

~~~sql
SELECT job_id,MAX(salary) `最高工资`
FROM employees
WHERE commission_pct IS NULL
GROUP BY job_id
HAVING `最高工资` > 6000
ORDER BY `最高工资` ASC;
~~~

先按照工种分类，查询每个工种没有奖金的员工的最高工资；筛选刚才的 结果，看哪个最高工资>6000；按最高工资升序。

#### 按多个字段分组

案例：查询每个工种每个部门的最低工资，并按最低工资降序

​	多个分组字段用**逗号隔开**，两个字段没有顺序要求。

~~~sql
SELECT MIN(salary) `最低工资`,job_id,department_id
FROM employees
GROUP BY job_id,department_id
ORDER BY `最低工资` DESC;
~~~

### 进阶六：连接查询

含义：又称多表查询，当查询的字段来自多个表时，就会用到连接查询。

如果直接使用以下语法，相当于用第一张表每一行内容去匹配第二张表每行内容，这样均能匹配成功。

~~~sql
select name,boyName from beauty,boys;
~~~

这样一个人的“理想男友”会变成多个。即为笛卡尔乘积现象，两个表所有行数相乘。两个表实现完全连接。

笛卡尔成绩现象：表1有m行，表2有n行，结果=m*n行

发生原因：没有有效的连接条件

如何避免：添加有效的连接条件

~~~sql
SELECT `name`,boyName FROM beauty,boys
WHERE beauty.`boyfriend_id` = boys.`id`;
~~~

用点号来标识属性的归属。

分类：

- 按年代分类
  - sql92标准：MySQL中仅仅支持内连接
  - sql99标准【推荐】：支持内连接+外连接（左外和右外）+交叉连接

- 按功能分类

  - 内连接
    - 等值连接
    - 非等值连接
    - 自连接

  - 外连接
    - 左外连接
    - 右外连接
    - 全外连接

  - 交叉连接

#### sql92标准

##### 等值连接

1、等值连接

语法：

select 查询列表

from 表1 别名，表2，别名

where 表1.key=表2.key

【and 筛选条件】

【group by 分组字段】

【having 分组后的筛选】

【order by 排序字段】

特点：

- 多表等值连接额结果为多表的交集部分
- n表连接，至少需要n-1个连接条件
- 多表的顺序没有要求
- 一般需要为表起别名
- 可以搭配前面介绍的所有子句使用，如筛选，分组，排序

案例1：查询女神名和对应的男神名

~~~sql
SELECT `name`,boyName 
FROM beauty,boys
WHERE beauty.`boyfriend_id` = boys.`id`;
~~~

将要查询的两个属性写在select中，查询的表写在from中，用逗号隔开，最重要的是写where语句。

用第一个表的每一行去匹配第二个表的每一行。

案例2：查询员工名和对应的部门名

~~~sql
SELECT last_name,department_name
FROM employees,departments
WHERE employees.`department_id` = departments.`department_id`;
~~~

2、为表起别名

- 提高语句的简洁度
- 区分多个重名的字段

注意：如果为表起了别名，则查询的字段就不能使用原来的表名去限定。

案例：查询员工名，工种号，工种名

~~~sql
SELECT last_name,employees.`job_id`,job_title
FROM employees,jobs
WHERE employees.`job_id` = jobs.`job_id`;
~~~

如果一个字段在多个表中都出现，为了避免歧义，那么需要加上完整的表名，为了方便，给表起别名

~~~sql
SELECT e.last_name,e.`job_id`,j.job_title
FROM employees e,jobs j
WHERE e.`job_id` = j.`job_id`;
~~~

**起了别名后，就不能再使用原来的名称了**，因为from先执行，这样表就有了别名。

3、两个表的顺序是否可以调换

可以替换

~~~sql
SELECT e.last_name,e.`job_id`,j.job_title
FROM jobs j,employees e
WHERE e.`job_id` = j.`job_id`;
~~~

表的交集

4、加筛选

案例1：查询有奖金的员工名、部门名

~~~sql
SELECT last_name,department_name
FROM employees e,departments d
WHERE e.`department_id`=d.`department_id`
AND e.`commission_pct` IS NOT NULL;
~~~

在WHERE后利用AND添加筛选

案例2：查询城市名中第二个字符为o的部门名和城市名

~~~sql
SELECT department_name,city
FROM `departments` d,`locations` l
WHERE d.`location_id`=l.`location_id`
AND city LIKE '_o%';
~~~

关键：找到两个的连接关系。一般都给表起别名，更方便。

5、加分组

案例1：查询每个城市的部门个数

~~~sql
SELECT COUNT(*) `部门个数`,city
FROM departments d,locations l
WHERE d.`location_id`=l.`location_id`
GROUP BY city;
~~~

部门个数，为count(*)，部门个数跟城市在两个表中，利用location_id进行连接，然后每个城市，用城市来进行分组。

案例2：查询有奖金的每个部门的部门名和部门的领导编号和该部门的最低工资

~~~sql
SELECT department_name,d.manager_id,MIN(salary)
FROM `departments` d,`employees` e
WHERE d.`department_id` = e.`department_id`
AND e.`commission_pct` IS NOT NULL
GROUP BY department_name,manager_id;
~~~

先把要查询的写在select，然后来自的表，先加表格的连接条件，然后增加筛选，最后加上按照部门名和领导编号进行分组。

因为要查询的有两个列，不能确定这两个列是否一一对应，因此分组的时候最好将这两个组都添加上。

6、加排序

案例：查询每个工种的工种名和员工的个数，并且按员工个数降序

~~~sql
SELECT job_title,COUNT(*) `员工个数`
FROM `jobs` j,`employees` e
WHERE j.`job_id`=e.`job_id`
GROUP BY job_title
ORDER BY `员工个数` DESC;
~~~

7、三表连接

案例：查询员工名、部门名和所在的城市

~~~sql
SELECT last_name,department_name,city
FROM employees e,departments d,locations l
WHERE e.`department_id`=d.`department_id`
AND d.`location_id`=l.`location_id`;
~~~

##### 非等值连接

语法：

select 查询列表

from 表1 别名，表2，别名

where 非等值条件

【and 筛选条件】

【group by 分组字段】

【having 分组后的筛选】

【order by 排序字段】

案例1：查询员工的工资和工资级别

~~~sql
SELECT salary,grade_level
FROM employees e,job_grades j
WHERE e.`salary` BETWEEN j.`lowest_sal` AND j.`highest_sal`;
~~~

salary在最低和最高之间的范围即可。也可以在后面加AND用于筛选或者增加排序。

##### 自连接

案例：查询员工名和上级的名称

​	如员工有对应的领导编号，然后用领导编号再找为此编号的员工姓名。相当于**一个表和自己相连接**。

{% asset_img 自连接.png This is an example image %}

将一张表当成2张或更多来使用。

~~~sql
SELECT e.employee_id,e.last_name,m.employee_id,m.last_name
FROM employees e,employees m
WHERE e.`manager_id`=m.`employee_id`;
~~~

这时候就体现出了起别名的好处，增加了代码可读性。

### 练习题4

1、显示员工表的最大工资，工资平均值

~~~sql
select max(salary),avg(salary) from employees;
~~~

2、查询员工表的employee_id，job_id，last_name，按department_id降序，salary升序

~~~sql
select employee_id，job_id，last_name
from employees
order by department_id desc,salary asc;
~~~

3、查询员工表的job_id中包含a和e的，并且a在e的前面

~~~sql
select job_id
from employees
where job_id like '%a%e%';
~~~

4、已知表student，里面有id(学号)，name,grade_id(年级编号)；已知表grade，里面有id(年级编号),name(年级名)；已知表result，里面有id，score，studentNo(学号)。要求查询姓名、年级名、成绩。

~~~sql
select s.name,g.name,r.score
from student s,grade g,result r
where s.grade_id=g.id
and s.id=r.studentNo;
~~~

5、显示当前日期，以及去前后空格，截取子字符串的函数

~~~
select now();
select trim(字符 from '');
select substr(str,startIndex);
select substr(str,startIndex,length);
~~~

6、显示所有员工的姓名，部门号和部门名称

~~~sql
SELECT last_name,d.department_id,department_name
FROM employees e,departments d
WHERE e.`department_id`=d.`department_id`;
~~~

7、查询90号部门员工的job_id和90号部门的location_id

~~~sql
SELECT e.job_id,d.location_id
FROM employees e,departments d
WHERE e.`department_id`=d.`department_id`
AND e.`department_id`=90;
~~~

限定条件为员工的部门编号为90

8、选择所有有奖金的员工的last_name,department_name,location_id,city

~~~sql
SELECT e.last_name,d.department_name,d.location_id,l.city
FROM employees e,departments d,locations l
WHERE e.`department_id`=d.`department_id`
AND d.`location_id`=l.`location_id`
AND e.`commission_pct` IS NOT NULL;
~~~

9、选择city在Toronto工作的员工的last_name，job_id，department_id，department_name

~~~sql
SELECT e.last_name,e.job_id,e.department_id,d.department_name
FROM employees e,departments d,locations l
WHERE e.`department_id`=d.`department_id`
AND d.`location_id`=l.`location_id`
AND l.`city`='Toronto';
~~~

10、查询每个工种、每个部门的部门名、工种名和最低工资

~~~sql
SELECT d.department_name,j.job_title,MIN(salary) `最低工资`
FROM departments d,jobs j,employees e
WHERE e.`department_id`=d.`department_id`
AND e.`job_id`=j.`job_id`
GROUP BY j.job_title,d.department_name;
~~~

11、查询每个国家下的部门个数大于2的国家编号

将位置表和部门表连接，这时候部门编号没有重复，每个部门编号都有对应的国家名，然后利用国家分组，就可以算出其部门个数。

{% asset_img 国家对部门表.png This is an example image %}

最后加上分组后的筛选条件，放在having语句后

~~~sql
SELECT country_id,COUNT(*) `部门个数`
FROM locations l,departments d
WHERE d.`location_id`=l.`location_id`
GROUP BY country_id
HAVING COUNT(*)>2;
~~~

having后也可以支持别名，但不太建议。

12、选择指定员工的姓名，员工号，以及他的管理者的姓名和员工号

格式类似于

employees 	Emp#		manager			Mgr#

~~~sql
SELECT e.last_name employees,e.employee_id `Emp#`,m.last_name manager,m.employee_id `Mgr#`
FROM employees e,employees m
WHERE e.`manager_id`=m.`employee_id`;
~~~

#### sql99标准

语法：

select 查询列表

from 表1 别名【连接类型】

join 表2 别名

on连接条件

【where 筛选条件】

【group by 分组】

【having 筛选条件】

【order by 排序列表】

对比sql92，将连接条件和筛选条件进行了分类，提高可读性

分类：

- **内连接**（重点）：inner
- 外连接
  - **左外**（重点）：left 【outer】
  - **右外**（重点）：right【outer】
  - 全外：full【outer】

- 交叉连接：cross

##### 内连接

select 查询列表

from 表1 别名

inner join 表2 别名

on 连接条件

【join 表3 别名 on 连接条件】

可以**多个join相连接**。

特点：

- 添加排序、分组、筛选
- inner可以省略
- 筛选条件放在where后面，连接条件放在on后面，提高分离性，便于阅读
- inner join连接和sql92语法中的等值连接效果是一样，都是查询多表的交集

1、等值连接

案例1：查询员工名、部门名

~~~sql
SELECT last_name,department_name
FROM employees e
INNER JOIN departments d
ON e.`department_id`=d.`department_id`;
~~~

将两个表的位置调换顺序也可以

案例2：查询名字中包含e的员工名和工种名

~~~sql
SELECT last_name,job_title
FROM employees e
INNER JOIN jobs j
ON e.`job_id`=j.`job_id`
WHERE last_name LIKE '%e%';
~~~

案例3：查询部门个数>3的城市名和部门个数

~~~sql
SELECT city,COUNT(*) 部门个数
FROM locations l
INNER JOIN departments d
ON l.`location_id`=d.`location_id`
GROUP BY city
HAVING COUNT(*)>3;
~~~

可以分步来做，先按照城市来分组，然后按照部门个数来筛选

案例4：查询哪个部门的部门员工个数>3的部门名和员工个数，并按个数降序

~~~sql
SELECT department_name,COUNT(*) 员工个数
FROM employees e
INNER JOIN departments d
ON e.`department_id`=d.`department_id`
GROUP BY department_name
HAVING COUNT(*)>3
ORDER BY COUNT(*) DESC;
~~~

案例5：查询员工名、部门名、工种名，并按部门名排序

~~~sql
SELECT last_name,department_name,job_title
FROM employees e
INNER JOIN departments d ON e.`department_id`=d.`department_id`
INNER JOIN jobs j ON e.`job_id`=j.`job_id`
ORDER BY department_name;
~~~

三表连接，先写一个inner join 表1 别名 on 连接条件，然后再写一个inner join 表2 别名 on 连接条件。保证第三个表和前两个表中的**至少一个有连接条件**。

2、非等值连接

案例1：查询员工的工资级别

~~~sql
SELECT salary,grade_level
FROM employees e
JOIN job_grades g
ON e.`salary` BETWEEN g.`lowest_sal` AND g.`highest_sal`;
~~~

{% asset_img jobgrades.png This is an example image %}

案例2：查询工资级别的个数>2的个数，并且按工资级别降序

~~~sql
SELECT grade_level,COUNT(*) 工资级别个数
FROM employees e
JOIN job_grades g
ON e.`salary` BETWEEN g.`lowest_sal` AND g.`highest_sal`
GROUP BY grade_level
HAVING COUNT(*)>2
ORDER BY grade_level DESC;
~~~

3、自连接

案例：查询姓名中包含字符k员工的名字，上级的名字

~~~sql
SELECT e.last_name,m.last_name
FROM employees e
JOIN employees m
ON e.`manager_id`=m.`employee_id`
WHERE e.`last_name` LIKE '%k%';
~~~

##### 外连接

应用场景：用于查询一个表中有，另一个表没有的记录

特点：

1. 外连接的查询结果为主表中的所有记录
   - 如果从表中有和它匹配的，则显示匹配的值
   - 如果从表中没有和它匹配的，则显示null
   - 外连接查询结果=内连接结果+主表中有而从表没有的记录

2. 左外连接，left join左边的是左表

   右外连接，right join右边的是主表

3. 左外和右外交换两个表的顺序，可以实现同样的效果

4. 全外连接=内连接的结果+表1中有但表2没有的+表2中有但表1没有的

引入：查询男朋友不在男神表的女神名

之前的内连接查两个表的交集部分，因此无法直接用来实现这里的需求。

做外连接实现

~~~sql
SELECT b.name,bo.*
FROM beauty b
LEFT JOIN boys bo
ON b.`boyfriend_id`=bo.`id`;
~~~

{% asset_img 左查询1.png This is an example image %}

如果只想看没有男朋友不在表中的女神信息，可以加入筛选

~~~sql
SELECT b.name,bo.*
FROM beauty b
LEFT JOIN boys bo
ON b.`boyfriend_id`=bo.`id`
WHERE bo.`id` IS NULL;
~~~

选择id来判断是因为boys中的id为主键，不能为null。判断时最好选从表中的主键列。

右外连接

~~~sql
SELECT b.name,bo.*
FROM boys bo
RIGHT JOIN beauty b
ON b.`boyfriend_id`=bo.`id`
WHERE bo.`id` IS NULL;
~~~

案例1：查询哪个部门没有员工

左外

~~~sql
SELECT d.*,e.employee_id
FROM departments d
LEFT JOIN employees e
ON d.`department_id`=e.`department_id`
WHERE e.`employee_id` IS NULL;
~~~

查询的没有员工的部门，因此主表是部门，然后与员工表左连接，筛选条件为没有员工即员工的id为NULL。

右外

~~~sql
SELECT d.*,e.employee_id
FROM employees e
RIGHT JOIN departments d 
ON d.`department_id`=e.`department_id`
WHERE e.`employee_id` IS NULL;
~~~

##### 全外

~~~sql
select b.*,bo.*
from beauty b
full join boys bo
on b.boyfriend_id=bo.id
~~~

MySQL不支持

##### 交叉连接

~~~sql
SELECT b.*,bo.*
FROM beauty b
CROSS JOIN boys bo;
~~~

结果就为笛卡尔乘积

#### sql92与sql99

功能：sql99支持的更多

可读性：sql99实现连接条件和筛选条件的分离，可读性较高

下为内连接，左外，右外的比较

{% asset_img 三种连接.png This is an example image %}

下为左外，右外加筛选和全外的比较

{% asset_img 左外右外全外.png This is an example image %}

### 多表连接练习

1、查询编号>3的女神的男朋友信息，如果有则列出详细，如果没有，用null填充

~~~sql
SELECT b.name,b.`id`,bo.*
FROM beauty b
LEFT JOIN boys bo
ON b.`boyfriend_id`=bo.`id`
WHERE b.`id`>3;
~~~

2、查询哪个城市没有部门

~~~sql
SELECT city
FROM locations l
LEFT JOIN departments d
ON l.`location_id`=d.`location_id`
WHERE d.`department_id` IS NULL;
~~~

用从表的主键去判断

3、查询部门名为SAL或IT的员工信息

这两个部门可能没有员工，会外连接会比较全。

~~~sql
SELECT department_name,e.*
FROM departments d
LEFT JOIN employees e
ON d.`department_id`=e.`department_id`
WHERE d.`department_name` IN('SAL','IT');
~~~

如果用departments来当主表，信息会比较全，会将空的员工信息也显示上去。用d来当主表对比用e来当主表，会多出NULL的员工信息，因为有的部门中没有员工。想要哪个表信息更全，就用哪个表来当主键。差的是部门的员工信息，因此主要是看部门。如果用员工表当主表，会去掉空的员工信息。

### 进阶七：子查询

#### 含义：

出现在其他语句中的select语句，称为子查询或内查询

外部的查询语句，称为主查询或外查询

外面的语句可以是insert、update、delete、**select**等，一般select作为外面语句较多

#### 分类：

按子查询出现的位置：

- select后面
  - 仅仅支持标量子查询
- from后面
  - 支持表子查询
- **where或having后面**（重点）
  - **标量子查询**（单行）（重点）
  - **列子查询**（多行）（重点）
  - 行子查询（较少）
- exists后面（相关子查询）
  - 表子查询	

按结果集的行列数不同：

- 标量子查询（结果集只有一行一列）
- 列子查询（结果集有只一列多行）
- 行子查询（结果集有一行多列）
- 表子查询（结果集一般为多行多列）

#### where或having后面

1、标量子查询（单行子查询）

2、列子查询（多行子查询）

3、行子查询（多列多行）

特点：

- 子查询放在小括号内

- 子查询一般放在条件的右侧

- 标量子查询，一般搭配着单行操作符使用

  <  >  <=  >=  =  <>

- 列子查询，一般搭配多行操作符使用

  IN,ANY/SOME,ALL

- 子查询的的执行优先于主查询执行，主查询的条件用到了子查询的结果

1、标量子查询

案例1：谁的工资比Abel高

步骤1：查询Abel的工资

~~~sql
SELECT salary
FROM employees
WHERE last_name='Abel';
~~~

出来的结果为一行一列，因此为标量子查询

步骤2：查询员工的信息，满足salary大于步骤1结果

~~~sql
SELECT *
FROM employees
WHERE salary>(
	SELECT salary
	FROM employees
	WHERE last_name='Abel'
);
~~~

案例2：返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id和工资

步骤1：查询141号员工的job_id

~~~sql
SELECT job_id
FROM employees
WHERE employee_id=141;
~~~

步骤2：查询143号员工的salary

~~~sql
SELECT salary
FROM  employees
WHERE employee_id=143;
~~~

步骤3：查询员工的姓名，job_id和工资，要求job_id=步骤1，并且salary>步骤2

~~~sql
SELECT last_name,job_id,salary
FROM employees
WHERE job_id=(
	SELECT job_id
	FROM employees
	WHERE employee_id=141
) AND salary>(
	SELECT salary
	FROM  employees
	WHERE employee_id=143
);
~~~

案例3：返回公司工资最少的员工的last_name,job_id和salary

步骤1：查询公司的最低工资

步骤2：查询last_name,job_id和salary，要求salary=步骤1

~~~sql
SELECT last_name,job_id,salary
FROM employees
WHERE salary=(
	SELECT MIN(salary)
	FROM employees
);
~~~

案例4：查询最低工资大于50号部门最低工资的部门id和其最低工资

步骤1：查询50号部门最低工资

步骤2：查询每个部门的最低工资

步骤3：在步骤2基础上筛选，满足min(salary)>步骤1结果

~~~sql
SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id
HAVING (
    SELECT MIN(salary)
    FROM employees
    WHERE department_id=50
);
~~~

非法使用标量子查询

子查询结果不为一行一列，可能为空或者多行，都不可以。

2、列子查询（多行子查询）

- 使用多行
- 使用多行比较操作符

| **操作符**    | **含义**                       |
| ------------- | ------------------------------ |
| **IN/NOT IN** | 等于列表中的**任意一个**       |
| ANY\|SOME     | 和子查询返回的**某一个**值比较 |
| ALL           | 和子查询返回的**所有**值比较   |

a > any(10,15,20)，意思为a比其中某一个大即可，此时可以替换成min

a>all(10,15,20)，要大于所有，此时可以替换成max

案例1：返回location_id是1400或1700的部门中的所有员工姓名

步骤1：查询location_id是1400或1700的部门标号

最好去重

~~~sql
SELECT DISTINCT department_id
FROM departments
WHERE location_id IN(1400,1700)
~~~

步骤2：查询员工姓名，要求部门号是步骤1结果中的某一个

~~~sql
SELECT last_name
FROM employees
WHERE department_id IN(
    SELECT DISTINCT department_id
    FROM departments
    WHERE location_id IN(1400,1700)
);
~~~

也可以将IN替换为= ANY，因为IN意思为在其中某个就可以，与= ANY意思相同。如果是NOT IN，表示全不在这个范围内，那么可以替换为NOT ALL，所有的都不在。

案例2：返回其它工种中比job_id为‘IT_PROG’工种任一工资低的员工的员工号、姓名、job_id 以及salary

步骤1：查询job_id为‘IT_PROG’部门任一工资

~~~sql
SELECT DISTINCT salary
FROM employees
WHERE job_id='IT_PROG'
~~~

步骤2：查询员工号、姓名、job_id 以及salary，salary<any（步骤1结果），job_id不为IT

~~~sql
SELECT employee_id,last_name,job_id,salary
FROM employees
WHERE salary<ANY(
	SELECT DISTINCT salary
	FROM employees
	WHERE job_id='IT_PROG'
) AND job_id <> 'IT_PROG';
~~~

或者小于其中任意工资，就是小于其最大工资

~~~sql
SELECT employee_id,last_name,job_id,salary
FROM employees
WHERE salary<(
	SELECT MAX(salary)
	FROM employees
	WHERE job_id='IT_PROG'
) AND job_id <> 'IT_PROG';
~~~

案例3：返回其它工种中比job_id为‘IT_PROG’工种所有工资都低的员工的员工号、姓名、job_id 以及salary

相比于案例2，相当于把any替换为all

~~~sql
SELECT employee_id,last_name,job_id,salary
FROM employees
WHERE salary<ALL(
	SELECT DISTINCT salary
	FROM employees
	WHERE job_id='IT_PROG'
) AND job_id <> 'IT_PROG';
~~~

或者小于所有，就是小于最小工资

~~~sql
SELECT employee_id,last_name,job_id,salary
FROM employees
WHERE salary<(
	SELECT Min(salary)
	FROM employees
	WHERE job_id='IT_PROG'
) AND job_id <> 'IT_PROG';
~~~

3、行子查询（结果集一行多列或多行多列）

案例：查询员工编号最小并且工资最高的员工信息

之前的写法

步骤1：查询最小的员工编号

~~~sql
SELECT MIN(employee_id) 
FROM employees
~~~

步骤2：查询最高工资

~~~sql
SELECT MAX(salary) 
FROM employees
~~~

步骤3：查询员工信息

~~~sql
SELECT *
FROM employees
WHERE employee_id=(
	SELECT MIN(employee_id) 
	FROM employees
) AND salary=(
	SELECT MAX(salary) 
	FROM employees
);
~~~

利用行子查询

~~~sql
SELECT *
FROM employees
WHERE (employee_id,salary)=(
	SELECT MIN(employee_id),MAX(salary)
	FROM employees
)
~~~

比较有局限性，要求查询的多个结果能用相同的运算符计算得出。

#### select后面

案例1：查询每个部门的员工个数

~~~sql
SELECT d.*,(
	SELECT COUNT(*)
	FROM employees e
	WHERE e.`department_id`=d.`department_id`
) `个数`
FROM departments d;
~~~

查询员工个数，然后显示在select后，因为要查询的员工数有要求和部门数id相同，需要加上筛选

如果用**左外连接**来做，如果直接查询count(*）这样查询到的员工数最少为1，错误将员工数为0的部门员工数查询为1，因为忽略了NULL值，应该使用

~~~sql
SELECT d.*,COUNT(employee_id)
FROM departments d
LEFT JOIN employees e
ON d.`department_id`=e.`department_id`
GROUP BY d.`department_id`
~~~

案例2：查询员工号=102的部门名

~~~sql
SELECT (
	SELECT department_name
	FROM departments d
	JOIN employees e
	ON d.`department_id`=e.`department_id`
	WHERE e.`employee_id`=102
) `部门名`;
~~~

没必要多加一层select。可以被其他方式代替

#### from后面

将子查询结果充当一张表，要求必须起别名

案例：查询每个部门的平均工资的工资等级

步骤1：查询每个部门的平均工资

~~~sql
SELECT AVG(salary),department_id
FROM employees
GROUP BY department_id
~~~

这样就得到了平均工资的表

步骤2：连接步骤1的结果集和job_grades表，筛选平均工资在最低和最高之间

~~~sql
SELECT ag_dep.*,g.`grade_level`
FROM (
	SELECT AVG(salary) ag,department_id
	FROM employees
	GROUP BY department_id
) ag_dep
INNER JOIN job_grades g
ON ag_dep.ag BETWEEN g.`lowest_sal` AND g.`highest_sal`;
~~~

注意，步骤1得到的表需要起别名，要用到的平均工资也起别名，将1得到的表和登记表内连接，然后输出1的所有信息和等级表的等级。

#### exists后面（相关子查询）

语法：

exists（完整的查询语句）

结果：1或0

~~~sql
SELECT EXISTS(SELECT employee_id FROM employees);
~~~

输出为1，表示子查询有值。输出0，表示子查询没有值

案例 1：查询有员工的部门名

~~~sql
SELECT department_name
FROM departments d
WHERE EXISTS(
	SELECT *
	FROM employees e
	WHERE e.`department_id`=d.`department_id`
);
~~~

或者使用WHERE后加IN

~~~sql
SELECT department_name
FROM departments d
WHERE d.`department_id` IN(
	SELECT department_id
	FROM employees
);
~~~

案例2：查询没有女朋友的男神信息

in的方式

~~~sql
select bo.*
from boys bo
where bo.id not in(
	select boyfiend_id
	from beauty
);
~~~

exists方式

~~~sql
select bo.*
from boys bo
where not exists(
	select boyfriend_id
	from beauty b
	where bo.id=b.boyfriend_id
);
~~~

### 练习题5

1、查询和Zlotkey相同部门的员工姓名和工资

步骤1：查询Zlotkey的部门

步骤2：查询部门号=步骤1的姓名和工资 

~~~sql
SELECT last_name,salary
FROM employees
WHERE department_id=(
	SELECT department_id
	FROM employees
	WHERE last_name='Zlotkey'
);
~~~

2、查询工资比公司平均工资高的员工的员工号，姓名和工资

步骤1：查询平均工资

步骤2：查询工资大于步骤1的员工号，姓名和工资

~~~sql
select employee_id,last_name,salary
from employees
where salary>(
	SELECT AVG(salary)
	FROM employees
);
~~~

3、查询各部门中工资比本部门平均工资高的员工的员工号，姓名和工资

步骤1：查询各部门的平均工资

步骤2：连接步骤1结果集和employee表，进行筛选

~~~sql
SELECT employee_id,last_name,e.salary,`avg`
FROM employees e
JOIN (
	SELECT department_id dep,AVG(salary) `avg`
	FROM employees
	GROUP BY department_id
) avg_dep
ON e.department_id=dep
WHERE e.salary > `avg`;
~~~

技巧：先要自己知道大概是什么样的结果，再写了去验证

4、查询和姓名中包含字母u的员工在相同部门的员工的员工号和姓名

步骤1：查询姓名中包含u的员工部门（去重）

步骤2：查询部门号=步骤1中任意一个的员工号和姓名

~~~sql
SELECT department_id,last_name
FROM employees
WHERE department_id IN(
	SELECT DISTINCT department_id
	FROM employees
	WHERE last_name LIKE '%u%'
);
~~~

5、查询在部门的location_id为1700的部门工作的员工的员工号

步骤1：查询location_id为1700的部门

步骤2：查询部门号=步骤1中任意一个的员工号

~~~sql
SELECT employee_id
FROM employees
WHERE department_id IN(
	SELECT DISTINCT department_id
	FROM departments
	WHERE location_id=1700
);
~~~

6、查询管理者是King的员工姓名和工资

步骤1：查询姓名为King的员工编号

步骤2：查询哪个员工 的manager_id=步骤1

当不知道用in还是=的时候，用in即可。

~~~sql
SELECT last_name,salary
FROM employees
WHERE manager_id IN(
	SELECT employee_id
	FROM employees
	WHERE last_name='K_ing'
);
~~~

7、查询工资最高的员工的姓名，要求first_name和last_name显示为一列，列名为姓.名

步骤1：查询最高工资

步骤2：查询工资=步骤1的结果的姓，名

~~~sql
select concat(first_name,'.',last_name) `姓.名`
from employees
where salary=(
	SELECT MAX(salary)
	FROM  employees
);
~~~

### 进阶八：分页查询

应用场景：当要显示的数据，一页显示不全，需要分页提交sql请求

语法：

~~~sql
select 查询列表
from 表
【join type  join 表2
on 连接条件
where筛选条件
group by分组字段
having 分组后的筛选
order by 排序的字段】
limit 【offset】,size;
offset：要显示条目的起始索引（起始索引从0开始）
size：要显示的条目个数
~~~

特点：

- limit语句放在查询语句的最后，执行也是最后，select–order by–limit

- 公式

  要显示的页数page，每页的条目数size

  select 查询列表

  from 表

  limit (page-1)*size,size

案例1：查询前五条员工信息

~~~sql
SELECT * FROM employees LIMIT 0,5;
SELECT * FROM employees LIMIT 5;
~~~

offset是可选的

案例2：查询第11条-25条

~~~sql
SELECT * FROM employees LIMIT 10,15;
~~~

案例3：有奖金的员工信息，并且工资较高的前10名显示

~~~sql
SELECT * FROM employees 
WHERE commission_pct IS NOT NULL
ORDER BY salary DESC
LIMIT 10;
~~~

### 练习题6

已知表 stuinfo，包含有以下字段

- id	学号
- name姓名
- email 邮箱 john@126.com
- gradeId 年级编号
- sex 性别 男/女
- age 年龄

已知表grade

- id 年级编号
- gradeName 年级名称

1、查询所有学员邮箱的用户名（注：邮箱中@前面的字符）

select substr(email,1,instr(email,’@’)-1)

from stuinfo;

用到的函数有截取字符串，获取指定字符在字符串中的位置

2、查询男生和女生的个数

select count(*) 个数,sex

from stuinfo

group by sex;

主要为对性别进行分组，然后计数

3、查询年龄>18岁的所有学生的姓名和年级名称

select name,gradeName

from stuinfo s

inner join grade g

on s.gradeId=g.id

where age>18;

考察连接查询

4、查询哪个年级的学生最小年龄>20

步骤1：查询每个年级的最小年龄

select min(age),gradeId

from stuinfo

group by gradeId

步骤2：在步骤1的结果上进行筛选，因此直接加上having判断即可

select min(age) ,gradeId

from stuinfo

group by gradeId

having min(age)>20;

5、说出查询语句中涉及到的所有的关键字，以及执行先后顺序

select 查询列表

from 表												

连接类型 join 表2

on	连接条件

where	筛选条件

group by	分组列表

having	分组后的筛选

order by	排序列表

limit 偏移,条目数;

执行顺序：1、from锁定数据源；2、join两表连接生成笛卡尔乘积表；3、on将满足条件的数据过滤出生成新表；4、where在3的基础上筛选生成新表；5、group by分组生成新的虚拟表格；6、having将分组后的表格进行筛选；7、select将筛选完毕后的表选出进行查看；8、order by进行排序；9、limit记性分页显示。

注意：**having后执行的是select**！！！**select在order by前面**！！！

6、查询生日在“1988-1-1”后的学生姓名、专业名称

~~~sql
SELECT `studentname`,`majorname`
FROM `student` s
JOIN `major` m
ON s.`majorid`=m.`majorid`
WHERE DATEDIFF(`borndate`,'1988-1-1')>0;
~~~

生日在某个日期之后，可以使用DATEDIFF函数来判断。或者使用WHERE borndate>’1988-1-1';也可以。

7、**查询每个专业的男生人数和女生人数分别是多少**

方式一：

按照专业和性别进行分组，**多个分组条件用逗号隔开**

~~~sql
SELECT COUNT(*) 个数,sex,`majorid`
FROM `student`
GROUP BY sex,`majorid`
~~~

但是这样显示效果不好，不是非常直观

方式二：

希望显示的效果是专业，男生，女神

因此基本语法应该为

select majorid,男，女

from student

group by major id;

然后男生，女生个数 可以使用子查询。如果直接使用select count(*) from student where sex=‘男’或者女，这样查到的人数是总体的，而并不是某个年级的。因此还需要附加条件，加上majorid的限制。

~~~sql
SELECT majorid,
(SELECT COUNT(*) FROM student WHERE sex='男' AND majorid=s.majorid) 男,
(SELECT COUNT(*) FROM student WHERE sex='女' AND majorid=s.majorid) 女
FROM student s
GROUP BY majorid;
~~~

{% asset_img 男生女生.png This is an example image %}

8、查询专业和张翠山一样的学生的最低分
步骤1：查询张翠山的专业

步骤2：查询学生最低分，筛选条件为专业为步骤1结果

~~~sql
SELECT MIN(score)
FROM `student` s
JOIN `result` r
ON s.`studentno`=r.`studentno`
WHERE s.`majorid`=(
	SELECT majorid
	FROM student
	WHERE `studentname`='张翠山'
);
~~~

或者可以先查张翠山专业，然后查哪些编号学生在这个专业中，然后查这些学生的最低分

~~~sql
SELECT MIN(score)
FROM result
WHERE `studentno` IN(
	SELECT `studentno`
	FROM `student`
	WHERE `majorid`=(
		SELECT majorid
		FROM student
		WHERE `studentname`='张翠山'
	)
);
~~~

9、查询大于60分的学生的姓名、密码、专业名

使用sql92写法

~~~sql
SELECT `studentname`,`loginpwd`,`majorname`
FROM `student` s,`result` r,`major` m
WHERE s.`studentno`=r.`studentno`
AND s.`majorid`=m.`majorid`
AND r.`score`>60;
~~~

使用sql99写法

~~~sql
SELECT `studentname`,`loginpwd`,`majorname`
FROM `student` s
JOIN `major` m ON s.`majorid`=m.`majorid`
JOIN `result` r ON s.`studentno`=r.`studentno`
WHERE r.`score`>60;
~~~

建议用99写法，更加直观，可读性较高。

10、**按邮箱位数分组，查询每组的学生个数**

~~~sql
SELECT COUNT(*),LENGTH(email)
FROM student
GROUP BY LENGTH(email);
~~~

**group by后面也可以接函数**。

11、查询学生名、专业名、分数

~~~sql
SELECT `studentname`,`majorname`,`score`
FROM `student` s
LEFT JOIN `major` m ON s.`majorid`=m.`majorid`
LEFT JOIN `result` r ON s.`studentno`=r.`studentno`
~~~

因为有的人分数为空，为了显示所有情况，使用左连接。

12、查询哪个专业没有学生，分别用左连接和右连接实现

~~~sql
SELECT m.`majorid`,m.`majorname`,s.`studentno`
FROM major m
LEFT JOIN student s
ON s.`majorid`=m.`majorid`
WHERE s.`studentno` IS NULL;
~~~

专业表为主表，为了显示没有学生即null的情况，需要使用左连接或者右连接（一个表有另一个没有），筛选条件为学生的主键为NULL。

13、查询没有成绩的学生人数

~~~sql
SELECT COUNT(*) 学生人数
FROM student s
LEFT JOIN `result` r
ON s.`studentno`=r.`studentno`
WHERE r.`id` IS NULL;
~~~

先将没有成绩学生查出来，即用左连接，学生为主表，筛选条件为成绩的主键为空，然后在此基础上统计人数。

### 子查询经典案例

1、查询工资最低的员工信息：last_name，salary

步骤1：查询最低工资

步骤2：查询员工信息，要求salary=最低工资

~~~sql
SELECT last_name,salary
FROM employees
WHERE salary=(
	SELECT MIN(salary)
	FROM employees
);
~~~

2、查询平均工资最低的部门信息

方式一：

步骤1：查询各部门的平均工资

步骤2：查询最低的平均工资

步骤3：查询部门编号等于最低平均工资的

步骤4：查询此部门信息

过于麻烦和啰嗦

方式二：

步骤1：查询各部门的平均工资

步骤2：求出最低平均工资的部门编号

~~~sql
SELECT department_id
FROM employees
GROUP BY department_id
ORDER BY AVG(salary)
LIMIT 1
~~~

步骤3：查询部门信息

~~~sql
SELECT *
FROM departments
WHERE department_id = (
	SELECT department_id
	FROM employees
	GROUP BY department_id
	ORDER BY AVG(salary)
	LIMIT 1
);
~~~

思路：在得到部门与平均工资表后，按照平均工资排序并只要最小的，得到部门编号，效率较高。

方式三：

步骤1：查询各部门的平均工资

步骤2：查询平均工资最低的部门信息

~~~sql
SELECT *
FROM departments d
INNER JOIN (
SELECT AVG(salary) `平均工资`,department_id
FROM employees
GROUP BY department_id
) avg_dep ON d.department_id=avg_dep.department_id
ORDER BY `平均工资`
LIMIT 1;
~~~

思路：得到表后与部门表连接，然后排序后选出1条，效率较低。

3、查询平均工资最低的部门信息和该部门的平均工资

查询出最低工资及部门编号的表，和部门表进行连接输出

~~~sql
SELECT d.*,ag
FROM departments d
JOIN (
	SELECT AVG(salary) ag,department_id
	FROM employees
	GROUP BY department_id
	ORDER BY AVG(salary)
	LIMIT 1
) avg_dep 
ON d.department_id=avg_dep.department_id;
~~~

表子查询及连接的方式

4、查询平均工资最高的job信息

步骤1：查询每个job的平均工资（先排序，再limit）

步骤2：找到最高的平均工资的job_id

步骤3：得到此job信息

~~~sql
SELECT *
FROM jobs
WHERE job_id = (
	SELECT job_id
	FROM employees
	GROUP BY job_id
	ORDER BY AVG(salary) DESC
	LIMIT 1
);
~~~

5、查询平均工资高于公司平均工资的部门有哪些

步骤1：查询公司平均工资

步骤2：查询每个部门的平均工资表，筛选条件为工资大于平均工资

~~~sql
SELECT AVG(salary) ag,department_id
FROM employees
GROUP BY department_id
HAVING ag >(
	SELECT AVG(salary)
	FROM employees
);
~~~

6、查询出公司中所有manager的详细信息

方式一：

两个表自连接，输出m表的信息

~~~sql
SELECT DISTINCT m.*
FROM employees e
JOIN employees m
ON e.manager_id=m.employee_id;
~~~

方式二：

步骤1：查询出所有的manager_id

步骤2：查询员工信息，需要满足employee_id在步骤1的结果中

~~~sql
SELECT *
FROM employees
WHERE employee_id IN(
	SELECT DISTINCT manager_id
	FROM employees
);
~~~

查询结果：自连接更快

7、各个部门中，最高工资中最低的那个部门的最低工资是多少 

步骤1：选出各个部门的最高工资

步骤2：找到最高工资最低的部门编号（order by,limit）

步骤3：得到id=步骤2的部门的最低工资

~~~sql
SELECT MIN(salary)
FROM employees
WHERE department_id = (
	SELECT department_id
	FROM employees
	GROUP BY department_id
	ORDER BY MAX(salary) ASC
	LIMIT 1
);
~~~

8、查询平均工资最高的部门的manager的详细信息：last_name，department_id，email，salary

方法一：

步骤1：查询平均工资最高的部门Id

步骤2：在departments表中找到id=步骤1结果的manager_id

步骤3：查询em_id=步骤2结果中的员工信息

~~~sql
SELECT last_name,department_id,email,salary
FROM employees
WHERE employee_id = (
	SELECT manager_id
	FROM departments
	WHERE department_id = (
		SELECT department_id
		FROM employees
		GROUP BY department_id
		ORDER BY AVG(salary) DESC
		LIMIT 1
	)
);
~~~

方法二：

步骤1：查询平均工资最高的部门Id

步骤2：将员工表和部门表进行连接，筛选条件为步骤1结果

~~~sql
SELECT last_name,d.department_id,email,salary
FROM employees e
JOIN departments d
ON e.employee_id=d.manager_id
WHERE e.department_id=(
	SELECT department_id
	FROM employees
	GROUP BY department_id
	ORDER BY AVG(salary) DESC
	LIMIT 1
);
~~~

两表连接效率比较低，执行时间比方式一更长。

### 进阶九：union联合查询

union：联合，合并，将多条查询语句结果合成一个结果

引入案例：查询部门编号>90或邮箱中包含a的员工信息

~~~sql
SELECT * FROM employees WHERE department_id>90 OR email LIKE '%a%';
~~~

如果使用union

~~~sql
SELECT * FROM employees WHERE department_id>90 
UNION
SELECT * FROM employees WHERE email LIKE '%a%';
~~~

语法：

查询语句1

union

查询语句2

union

…

应用场景：要查询的结果来自多个表，且多个表没有直接的连接关系，但查询的信息一致时

特点：

1. 要求多条查询语句的查询列数是一致的
2. 要求多条查询语句的查询的每一列的类型和顺序最好是一致的
3. 使用union关键字默认去重，如果使用**union all**可以包含重复项

### 查询总结

select 查询列表                       7

from 表1 别名                         1

连接类型 join 表2 别名          2

on 连接条件                            3

where 筛选                             4

group by 分组列表                 5

having 筛选                            6

order by 排序列表                 8

limit offset,limit;                    9

group by和having别名使用相对较少。select *输出全部信息，但实际为了可读性，最好写完整对应列。

## DML语言

数据操作语言

- 插入：insert
- 修改：update
- 删除：delete

### 插入语句

#### 方式一：经典插入

语法：

insert into 表名(列名,…) values(值1,…),(值2,…),…;

1、插入的值的类型要与列的类型一致或兼容

~~~sql
INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id) 
VALUES(13,'唐艺昕','女','1990-4-23','18988888888',NULL,2);
~~~

如果插入数据是‘1234’也可以插入到int列中，可以隐式转换。

2、不可以为null的列必须插入值，可以为null的列如何插入值

方式一：值直接写null

~~~sql
INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id) 
VALUES(13,'唐艺昕','女','1990-4-23','18988888888',NULL,2);
~~~

方式二：列和值均不写

~~~sql
INSERT INTO beauty(id,NAME,sex,borndate,phone,boyfriend_id) 
VALUES(14,'金星','女','1990-4-23','13888888888',2);
~~~

photo列和值均被省略

3、列的顺序可以调换，但要一一对应

~~~sql
INSERT INTO beauty(NAME,sex,id,phone)
VALUES('蒋欣','女',16,'123');
~~~

4、列数和值的个数必须一致

5、可以省略列名，默认所有列，而且列的顺序和表中的顺序一致

~~~sql
INSERT INTO beauty
VALUES(18,'张飞','男',NULL,'1212',NULL,NULL);
~~~

#### 方式二：

语法：

insert into 表名

set 列名=值,列名=值,…

~~~sql
INSERT INTO beauty
SET id=19,NAME='刘涛',phone='9001';
~~~

#### 两种方式比较

1、方式一支持插入多行，方式二不支持

~~~sql
INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id) 
VALUES(23,'唐艺昕1','女','1990-4-23','18988888888',NULL,2)
,(24,'唐艺昕2','女','1990-4-23','18988888888',NULL,2)
,(25,'唐艺昕3','女','1990-4-23','18988888888',NULL,2);
~~~

在values后用逗号隔开，然后加括号写要插入的值。

2、方式一支持子查询，方式二不支持

~~~sql
INSERT INTO beauty(id,NAME,phone) 
SELECT 26,'宋茜','183298';
~~~

这样也可以实现插入

~~~sql
INSERT INTO beauty(id,NAME,phone) 
SELECT id,boyname,'1423432'
FROM boys
WHERE id<3;
~~~

### 修改语句

#### 修改单表的记录【重要】

语法：

update 表名

set 列=新值，列=新值,…

where 筛选条件;

案例1：修改beauty表中姓唐的女神电话为13888888888

~~~sql
UPDATE beauty SET phone='13888888888'
WHERE NAME LIKE '唐%'; 
~~~

案例2：修改boys表中id号为2的名称为张飞，魅力值为10

~~~sql
UPDATE boys SET boyName='张飞',userCp=10
WHERE id=2;
~~~

#### 修改多表的记录【补充】

语法：

sql92语法

update 表1 别名，表2 别名

set 列=值,….

where 连接条件

and 筛选条件;

sql99语法

**update 表1 别名**

**inner|left|right|full join 表2 别名**

**on 连接条件**

**set 列=值,….**

**where 筛选条件;**

案例1：修改张无忌的女朋友的手机号为114

~~~sql
UPDATE boys bo
INNER JOIN beauty b
ON bo.`id`=b.`boyfriend_id`
SET b.`phone`='114'
WHERE bo.`boyName`='张无忌';
~~~

案例2：修改没有男朋友的女神的男朋友编号都为2号

~~~sql
UPDATE boys bo
RIGHT JOIN beauty b
ON bo.`id`=b.`boyfriend_id`
SET b.`boyfriend_id`=2
WHERE bo.`id` IS NULL;
~~~

筛选条件为从表的主键为空

### 删除语句

方式一：delete

语法：

1. 单表的删除【重点】

   delete from 表名【 where 筛选条件】【limit 条目数】

   筛选条件是可选的，如果不写删除条件，就是删除整个表数据。

2. 多表的删除【补充】

   - sql92语法

     delete 删除表的别名 【，表2的别名，…】

     from 表1 别名，表2 别名

     where 连接条件

     and 筛选条件;

   - sql99语法

     delete 表1的别名【,表2的别名】

     from 表1 别名

     inner|left|right join 表2 别名 on 连接条件

     where 筛选条件;

方式二：truncate

语法：truncate table 表名

清空数据，不能加where，将表中信息全部删除

#### 方式一：delete

1、单表的删除

案例：删除手机号以9结尾的女神信息

~~~sql
DELETE FROM beauty WHERE phone LIKE '%9';
~~~

delete 和from 之间不能加列，删除的是一整行的信息。

2、多表 的删除

案例1：删除张无忌的女朋友的信息

~~~sql
#删除哪个表信息就delete那个表
DELETE b
FROM boys bo
INNER JOIN beauty b ON bo.`id`=b.`boyfriend_id`
WHERE bo.`boyName`='张无忌';
~~~

案例2：删除黄晓明的信息及他女朋友的信息

~~~sql
DELETE bo,b
FROM beauty b
INNER JOIN boys bo ON b.`boyfriend_id`=bo.`id`
WHERE bo.`boyName`='黄晓明';
~~~

将要删除的表写在delete后面。

#### 方式二：truncate语句

案例：将魅力值>100的男神信息删除

~~~sql
TRUNCATE TABLE boys;
~~~

不能加where筛选，只能把表全部删除。

#### 两种方式比较【面试题，重要】

1. delete 可以加where条件，truncate不能加
2. truncate删除，效率高一点
3. 加入要删除的表中有自增长列，用**delete**删除后，再插入数据，**自增长列的值从断点开始**；而用**truncate**删除后，再插入数据，**自增长列的值从1**开始
4. **truncate**删除**没有返回值**，**delete**删除**有返回值**
5. truncate删除不能回滚，delete删除可以回滚

### 练习题

1、插入语法的两种方式

方式一：

~~~sql
INSERT INTO my_employees
VALUES (1,'patel','Ralph','Rpatel',895)
,(2,'Dancs','Betty','Bdancs',860)
,(3,'Biri','Ben','Bbiri',1100)
,(4,'Newman','Chad','Cnewman',750)
,(5,'Ropeburn','Audrey','Aropebur',1550);
~~~

方式二：

~~~sql
INSERT INTO my_employees
SELECT 1,'patel','Ralph','Rpatel',895 UNION
SELECT 2,'Dancs','Betty','Bdancs',860 UNION
SELECT 3,'Biri','Ben','Bbiri',1100 UNION
SELECT 4,'Newman','Chad','Cnewman',750 UNION
SELECT 5,'Ropeburn','Audrey','Aropebur',1550;
~~~

insert into后面可以跟select语句，利用union将多条select语句变为一条，最后一个select后不用加union。

2、将3号员工的last_name修改为“drelxer”

~~~sql
UPDATE my_employees
SET Last_name='drelxer'
WHERE id=3;
~~~

3、将userid 为Bbiri的user表和my_employees表的记录全部删除

~~~sql
DELETE my,us
FROM users us
JOIN my_employees my ON us.`userid`=my.`Userid`
WHERE us.`userid`='Bbiri';
~~~

4、检查表所做的修正

~~~sql
SELECT * FROM my_employees;
SELECT * FROM users;
~~~

## DDL语言

数据定义语言，库和表的管理

1、库的管理

创建、修改、删除

2、表的管理

创建、修改、删除

与数据操作语言区分，这里管理的是表的结构，而不是数据

- 创建：create
- 修改：alter
- 删除：drop

### 库的管理

#### 库的创建

语法：

create database 【if not exsits】库名 【character set 字符集名】;

案例：创建库books

~~~sql
CREATE DATABASE books;
~~~

但是这样创建完成后，再次执行会报错，采用下面语句提高语句容错性

~~~sql
CREATE DATABASE IF NOT EXISTS books;
~~~

#### 库的修改

**基本不修改**

库名不能使用sql语句去修改

可以更改库的字符集

~~~sql
ALTER DATABASE books CHARACTER SET gbk;
~~~

#### 库的删除

~~~sql
DROP DATABASE IF EXISTS books;
~~~

存在才删除

### 表的管理

#### 表的创建【重要】

create table 【if not exists】表名(

​	列名 列的类型【（长度）约束】，

​	列名 列的类型【（长度）约束】，

​	….

​	列名 列的类型【（长度）约束】

);

案例：创建表book

~~~sql
CREATE TABLE book(
	id INT,#编号
	bName VARCHAR(20),#书名
	price DOUBLE,#价格
	authorId INT,#作者编号
	publishDate DATETIME#出版日期
);
~~~

不同字段用逗号隔开，最后个不用加逗号。

#### 表的修改

**alter table 表名 add|drop|modify|change column 列名 【列类型 约束】;**

1、修改列名

alter table 表名 change column 旧列名 新列名 新列名类型

~~~sql
ALTER TABLE book CHANGE COLUMN publishDate pubDate DATETIME;
~~~

change column要求加上新的列名和类型。column可以省略。

2、修改列的类型或约束

alter table 表名 modify column 列名 新类型 【新约束】

~~~sql
ALTER TABLE book MODIFY COLUMN pubDate TIMESTAMP 【新约束】;
~~~

用到的为modify。

3、添加新列

默认添加在最后列，可以自己指定位置。在最开头，使用first，在某个字段后面，使用after字段名。

alter table 表名 add column 列名 类型 【first|after 字段名】

~~~sql
ALTER TABLE author ADD COLUMN annual DOUBLE 【first|after 字段名】;
~~~

用到的为add。同样需要加上类型。

4、删除列

alter table 表名 drop column 列名

~~~sql
ALTER TABLE book DROP COLUMN annual;
~~~

用到的为drop，这时候就不需要指定类型了。

5、修改表名

alter table 表名 rename 【to】新表名

~~~sql
ALTER TABLE author RENAME TO book_author;
~~~

#### 表的删除

~~~sql
DROP TABLE 【IF EXISTS】 book_author;
~~~

通用写法

~~~sql
DROP DATABASE IF EXISTS 旧库名;
CREATE DATABASE 新库名;

DROP TABLE IF EXISTS 旧库名;
CREATE TABLE 表名();
~~~

#### 表的复制

1、仅复制表的结构

create table 表名 like 旧表

~~~sql
CREATE TABLE copy LIKE author;
~~~

没有复制内容。

2、复制表的结构+数据

create table 表名

select 查询列表 from 表 【where 筛选】

~~~sql
CREATE TABLE copy2 
SELECT * FROM author;
~~~

后面跟select查询语句

复制部门数据

~~~sql
CREATE TABLE copy3
SELECT id,au_name
FROM author
WHERE nation='中国';
~~~

仅复制某些字段

~~~sql
CREATE TABLE copy4
SELECT id,au_name
FROM author
WHERE 0;
~~~

利用where 0来获取数据。

### 练习

1、将其他库中的表信息复制进当前库中的表

~~~sql
CREATE TABLE dept2
SELECT `department_id`,`department_name`
FROM `myemployees`.`departments`;
~~~

使用库名.表名来进行调用。利用create后面+select子查询。

2、复制其他库中表的结构

~~~sql
CREATE TABLE employees2 LIKE `myemployees`.employees;
~~~

3、删除表

~~~sql
DROP TABLE IF EXISTS emp5;
~~~

### 常见的数据类型

- 数值型
  - 整数
  - 小数
    - 定点数
    - 浮点数
- 字符型
  - 较短的文本：char,varchar
  - 较长的文本：text，blob（较长的二进制数据）
- 日期型

#### 原则

所选择的类型越简单越好，能保存数值的类型越小越好

#### 整型

分类：

tinyint（1字节）、smallint（2字节）、mediumint（3字节）、int/integer（4字节）、bigint（8字节）

特点：

1. 如果不设置有符号还是无符号，默认是有符号，如果要设置无符号，加上关键字unsigned

2. 如果插入的数值超出了整形的范围，会报out of range异常，并且插入临界值

   临界值为对应允许范围内最接近插入值的值如果tinyint范围是-128-127，插入-129，存入-128；插入128，存入127

3. 如果不设置长度，会有默认长度

   宽度效果为：如果数据长度没有达到指定长度，则在左方用0补齐，需要搭配关键字**zerofill**使用，此时整形默认变为**无符号**类型

1、设置无符号和有符号

~~~sql
CREATE TABLE tab_int (
	id INT,
	id2 INT UNSIGNED
);
~~~

#### 小数

分类

1. 浮点型
   - float(M,D)  4字节
   - double(M,D) 8字节
2. 定点型
   - dec(M,D)  M+2字节，最大取值范围与double相同
   - decimal(M,D)

特点：

1. M：整数部位+小数部位的总长度

   D：小数部位

   如果超过范围，则插入临界值

2. M和D都可以省略

   如果是decimal，则M默认是10，D默认是0

   如果是float和double，则会根据插入数值的精度来决定精度

3. 定点型的精确度较高，如果要求插入数值的精度较高，如货币运算则考虑使用

#### 字符型

分类：

较短的文本：char、varchar【必须加单引号！！！】

其他：

binary和varbiary用于保存较短的二进制

enum用于保存枚举

set用于保存集合

较长的文本：text、blob（较大的二进制数据）

特点：

##### char和varchar类型【重要】

| 字符串类型 | 最多字符数           | 描述及存储需求       | 空间耗费                   | 效率 |
| ---------- | -------------------- | -------------------- | -------------------------- | ---- |
| char(M)    | M，可以省略，默认为1 | M为0-255之间的整数   | 固定长度字符，比较耗费空间 | 高   |
| varchar(M) | M，不可省略          | M为0-65535之间的整数 | 可变长度字符，比较节省空间 | 低   |

M为字符，一个汉字或者一个英文字符均为1字符。char固定长度，代表所有字符均用统一长度存储；varchar可变长度，代表存储长度随数据具体情况而变化。如果字符长度较为固定，用char，如性别；如果字符长度变化较大，用varchar，如姓名。

##### binary和varbinary类型

类似于char和varchar，不同的是它们包含二进制字符串而不包含非二进制字符串

##### Enum类型

枚举类型，要求插入的值必须属于列表中指定的值之一。如果列表成员为1~255，则需要1个字节存储，如果列表成员为255-65535，则需要2个字节存储，最多需要65535个成员！

~~~sql
c1 Enum('a','b','c');
~~~

如果插入为对应的大写，保存为小写。如果插入类型不在枚举类型中，插入为空。

##### Set类型

说明：和ENUM类型类似，里面可以保存0~64个成员。和ENUM类型最大的区别是：SET类型一次可以选**取多个成员**，而ENUM只能选一个
根据成员个数不同，存储所占的字节也不同

~~~sql
s1 SET('a','b','c','d');
INSERT INTO tab_set values('a,b,c')
~~~

同样不区分大小写。

#### 日期型

分类：

date 只保存日期

time 只保存时间

year 只保存年

datetime 保存日期+时间

timestamp 保存日期+时间

特点：

|           | 字节 | 范围      | 时区的影响 |
| --------- | ---- | --------- | ---------- |
| datetime  | 8    | 1000-9999 | 不受       |
| timestamp | 4    | 1970-2038 | 受         |

| 日期和时间类型 | 字节 | 最小值              | 最大值              |
| -------------- | ---- | ------------------- | ------------------- |
| date           | 4    | 1000-01-01          | 9999-12-31          |
| datetime       | 8    | 1000-01-01 00:00:00 | 9999-12-31 23:59:59 |
| timestam       | 4    | 19700101080001      | 2038年的某个时刻    |
| time           | 3    | -838:59:59          | 838:59:59           |
| year           | 1    | 1901                | 2155                |

重点为datatime和timestam。

##### datetime和timestamp区别

1、**Timestamp**支持的时间**范围较小**，取值范围：
	19700101080001——2038年的某个时间
	Datetime的取值范围：1000-1-1 ——9999-12-31
2、timestamp和**实际时区**有关，更能反映**实际的日期**，而datetime则只能反映出**插入时的当地时区**
3、timestamp的属性受Mysql版本和SQLMode的影响很大

~~~sql
CREATE TABLE tab_date(
	t1 DATETIME,
	t2 TIMESTAMP
);
INSERT INTO tab_date VALUES(NOW(),NOW());
SELECT * FROM tab_date;
~~~

这时候出来结果相同，但是当更改时区后

~~~sql
SET time_zone='+9:00';#从东八更改为东九
~~~

这时候，timestamp类型数据存储的时间比datetime存储的时间快一个小时，随着时区而变化了。

### 常见约束

含义：一种限制，用于限制表中的数据，为了保证表中数据的准确和可靠性

分类：六大约束

- NOT NULL：非空，用于保证该字段的值不能为空

  如姓名，学号等

- DEFAULT：默认，用于保证该字段有默认值

  如性别

- PRIMARY KEY：主键，用于保证该字段的值有**唯一性**，且要**非空**

  如学号，员工编号等，可以多个列组合成一个主键

  primary key(id,stuname)，插入值要求不能所有组成主键的列相同

- UNIQUE：唯一，用于保证该字段的值具有唯一性，可以为空

  如座位号，但**最多只能有一个为空**

- CHECK：检查约束【mysql不支持】

  比如年龄，性别

- FOREIGN KEY：外键，用于限制两个表的关系，用于保证该字段的值必须来自于主表的关联列的值

  在从表添加外键约束，用于引用主表中某列的值

  如专业编号，员工表的部门编号，员工表的工种编号

**一个字段可以添加多个约束，用空格隔开，无顺序要求**

添加约束的时机：

1. 创建表时
2. 修改表时

约束的添加分类：

1. 列级约束

   六大约束语法上都支持，但外键约束没有效果

2. 表级约束

   除了**非空**，**默认**，其他都支持

CREATE TABLE 表名(

​	字段名 字段类型 列级约束，

​	字段名 字段类型，

​	表级约束

)

#### 列级约束与表级约束

|          | 位置       | 支持的约束类型             | 是否可以起约束名       |
| -------- | ---------- | -------------------------- | ---------------------- |
| 列级约束 | 列的后面   | 语法都支持，但外键没有效果 | 不可以                 |
| 表级约束 | 所有列下面 | 默认和非空不支持，其他支持 | 可以（对主键没有效果） |

#### 主键和唯一的对比【面试考 ，重要】

|      | 保证唯一性 | 是否允许为空       | 一个表中可以有多少个 | 是否允许组合 |
| ---- | ---------- | ------------------ | -------------------- | ------------ |
| 主键 | 是         | 否                 | 至多有1个            | 是，但不推荐 |
| 唯一 | 是         | 是，但最多一个为空 | 可以有多个           | 是，但不推荐 |

#### 外键

1. 要求在从表设置外键关系
2. 从表的外键列的类型和主表的关联列的类型要求一致或兼容，名称无要求
3. 主表的关联列**必须是一个key**（一般是**主键**或唯一）
4. 插入数据时，先插入主表，再插入从表
5. 删除数据时，先删除从表，再删除主表

可通过以下两种方式删除主表的记录

方式一：级联删除

一般直接删除主表的信息不行因为要先删除从表，但加上级联删除后，直接删除主表信息后，从表信息也会被删除。

在添加外键的约束后加上on delete cascade

~~~sql
alter table stuinfo add constraint fk_stu_major foreign key(majorid) references major(id) on delete cascade;
~~~

方式二：级联置空

如果不想删除主表信息时从表信息也被删除，就是用级联置空，主表信息被删除后，从表相应位置信息置空

在添加外键的约束后加上on delete set null

~~~sql
alter table stuinfo add constraint fk_stu_major foreign key(majorid) references major(id) on delete set null
~~~

#### 创建表时添加约束

1、添加列级约束

语法：

直接在字段名和类型后面追加约束类型即可

只支持：主键、唯一、默认、非空

~~~sql
CREATE TABLE stuinfo(
	id INT PRIMARY KEY,#主键
	stuName VARCHAR(20) NOT NULL,#非空
	gender CHAR(1) CHECK(gender IN ('男','女')),#检查
	seat INT UNIQUE,#唯一
	age INT DEFAULT 18,#默认约束
	majorId INT REFERENCES major(id)#外键
);
~~~

**外键在列级约束时没有用**。主键，外键，唯一键自动生成索引，使用下面语句查看索引

~~~sql
SHOW INDEX FROM stuinfo;
~~~

2、添加表级约束

语法：在各个字段的最下面，constraint 约束名可省略

【constraint 约束名】 约束类型（字段名）

~~~sql
CREATE TABLE stuinfo(
	id INT,
	stuName VARCHAR(20),
	gender CHAR(1),
	seat INT,
	age INT,
	majorId INT,
	
	CONSTRAINT pk PRIMARY KEY(id),#主键
	CONSTRAINT uk UNIQUE(seat),#唯一键
	CONSTRAINT ck CHECK(gender IN ('男','女')),#检查
	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)#外键
);
~~~

主键名是默认的，改了没有效果。

~~~
外键名一般为fk_当前表名_主键名
~~~

通用写法：

~~~sql
CREATE TABLE IF NOT EXISTS stuinfo(
	id INT PRIMARY KEY,
	stuname VARCHAR(20) NOT NULL,
	gender CHAR(1),
	age INT DEFAULT 18,
	seat INT UNIQUE,
	majorid INT,
	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)
);
~~~

#### 修改表时添加约束

总结：

1、添加列级约束

alter table 表名 **modify column** 字段名 字段类型 新约束；

2、添加表级约束

alter table 表名 **add** 【constraint 约束名】 约束类型（字段名）【外键的引用】；

1、添加非空约束

~~~sql
ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20) NOT NULL;
~~~

2、添加默认约束

~~~sql
ALTER TABLE stuinfo MODIFY COLUMN age INT DEFAULT 18;
~~~

3、添加主键

~~~sql
#1、列级约束
ALTER TABLE stuinfo MODIFY COLUMN id INT PRIMARY KEY;
#2、表级约束
ALTER TABLE stuinfo ADD PRIMARY KEY(id);
~~~

4、添加唯一键

~~~sql
#1、列级约束
ALTER TABLE stuinfo MODIFY COLUMN seat INT UNIQUE;
#2、表级约束
ALTER TABLE stuinfo ADD UNIQUE(seat);
~~~

5、添加外键

关键词为ADD，只有表级约束才有效果

~~~sql
ALTER TABLE stuinfo ADD FOREIGN KEY(majorid) REFERENCES major(id);
~~~

#### 修改表时删除约束

1、删除非空约束

~~~sql
ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20) NULL;
~~~

2、删除默认约束

不加约束即可

~~~sql
ALTER TABLE stuinfo MODIFY COLUMN age INT;
~~~

3、删除主键

~~~sql
ALTER TABLE stuinfo DROP PRIMARY KEY;
~~~

4、删除唯一

drop index 唯一键名

~~~sql
ALTER TABLE stuinfo DROP INDEX seat;
~~~

5、删除外键

drop foreign key 外键名

~~~sql
ALTER TABLE stuinfo DROP FOREIGN KEY fk_stuinfo_major;
~~~

### 标识列

又称为自增长列，关键字为auto_increament

含义：可以**不用手动的插入值**，系统提供默认的序列值，默认从1开始，步长为1

特点：

1. 标识列必须和**key搭配**

2. 一个表中**至多**有一个标识列

3. 标识列的类型只能是**数值型**

4. 标识列可以通过SET auto_increment_increment=2;设置步长

   可以通过手动插入值，设置起始值

一、创建表时设置标识列

~~~sql
CREATE TABLE tab_identity(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20)
);
INSERT INTO tab_identity VALUES(NULL,'john');
~~~

可以设置自增长的步长，一般不会改

~~~sql
SHOW VARIABLES LIKE '%auto_increment%';
SET auto_increment_increment=2;
~~~

二、修改表时设置标识列

~~~sql
ALTER TABLE tab_identity MODIFY id INT PRIMARY KEY AUTO_INCREMENT;
~~~

三、修改表时删除标识列

去掉自增长列标识即可

~~~sql
ALTER TABLE tab_identity MODIFY id INT;
~~~

## TCL语言

Transaction Control Language 事务控制语言

### 事务

事务：

一个或一组sql语句组成一个执行单元，这个执行单元要么全部执行，要么全部不执行。每个sql语句是相互依赖的。整个单独单元作为一个不可分割的整体，如果单元中某条sql语句一旦执行失败或产生错误，整个单元将会回滚。所有受到影响的数据将返回到事务开始以前的状态；如果单元中的所有sql语句均执行成功，则事务被顺利执行。

#### 了解存储引擎

1、概念：在mysql中的数据用各种不同的技术存储在文件（或内存）中。

2、通过show engines;来查看mysql支持的存储引擎

3、在mysql中用的最多的存储引擎有：innodb，myisam，memory等。其中innodb支持事务，而myisam，memory等不支持事务

#### 事务的ACID（acid）属性【重要】

事务的特性：

1. **原子性**（Atomicity）

   原子性指事务是一个**不可分割**的工作单位，事务中的操作要么都发生，要么都不发生

2. **一致性**（Consistency）

   事务必须使数据库从一个一致性状态变换到另外一个一致性状态，保证数据的**可靠性**

   数据要准确，可靠	

3. **隔离性**（Isolation）

   事务的隔离性是指一个事务的执行**不能被其他事务干扰**，即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。

4. **持久性**（Durability）

   持久性指一个事务一旦被提交，它对数据库中数据的改变就是**永久性**的，接下来的其他操作和数据库故障不应该对其有任何影响

#### 有关ACID的理解

> 学习知乎回答 https://www.zhihu.com/question/31346392

AID，是在数据库层面的特征，即依赖于数据库的具体实现，如A依靠数据库的undo log，I依靠MVCC，D依靠redo log，而C一致性，依靠的是应用层，即开发者。一致性的概念是系统从一个正确的状态迁移到另一个正确的状态，而是否正确，指需要满足预定的约束的状态，而这个状态是根据业务场景由开发者设定的。而ACID为通过AID来保障C一致性，AID是手段，C是目的。比如转账系统，一个人有50元，给其他人转100，变成-50，这个事务执行没有破坏数据库层面的约束，但是破坏了应用层的约束。

#### 事务的创建

**隐式事务**：事务没有明显的开启和结束的标记

比如insert、update、delete语句

~~~sql
delete from 表 where id = 1;
~~~

**显式事务**：事务具有明显的开启和结束的标记

前提：必须先设置自动提交功能为禁用

只针对当前会话有效

~~~sql
SHOW VARIABLES LIKE '%autocommit';
SET autocommit=0;
~~~

步骤1：开启事务

~~~sql
SET autocommit=0;
START TRANSACTION;#可选的
~~~

步骤2：编写事务中的sql语句（**select insert update delete**）

语句1；

语句2；

…

【设置回滚点：

savepoint 回滚点名;】

步骤3：结束事务

~~~sql
commit;提交事务
rollback;回滚事务
[回滚到指定的地方：rollback to 回滚点名;]
~~~

savepoint 节点名;#设置保存点

##### 演示事务的使用步骤

~~~sql
#1、开启事务
SET autocommit=0;
START TRANSACTION;
#2、编写sql语句
UPDATE account SET balance = 500 WHERE username='张无忌';
UPDATE account SET balance = 1500 WHERE username='赵敏';
#3、结束事务
COMMIT;
~~~

如果使用commit，可以正常修改数据，如果使用的是rollback，那么对数据没有影响

### 并发事务【重要】

对于同时运行的**多个事务**，当这些事务访问**数据库中相同的数据**时，如果没有采取必要的隔离机制，就会导致各种并发问题： 

- **更新丢失**：多个事务选择同一行，最后的更新会覆盖之前的更新。在提交事务前，不能访问同一文件，可避免。

- **脏读**：对于两个事务T1、T2，T1读取了已经被T2**更新**但**还没有提交**的字段，之后若T2回滚，T1读取的内容就是**临时且无效**的
- **不可重复读**：对于两个事务T1、T2，T1读取了一个字段，然后T2**更新**了该字段，之后T1再次读取同一个字段，**值就不同**了
- **幻读**：对于两个事务T1、T2，T1从一个表中读取了一个字段，然后T2在该表中**插入**了一些新的行，之后如果T1再次读取同一个表，就会多出几行

脏读与幻读相对比，脏读侧重于字段的**更新**，幻读侧重于行的**插入**。

#### 解决并发问题【重要】

**数据库的隔离级别**：数据库系统必须具有隔离并发运行各个事务的能力，使它们不会相互影响，避免各种并发问题

数据库提供的4种事务隔离级别

| 隔离级别                         | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| READ UNCOMMITTED（读未提交数据） | 允许事务读取未被其他事务提交的变更，**脏读、不可重复度和幻读**的问题都会出现 |
| READ COMMITTED（读已提交数据）   | 只允许事务读取已经被其他事务提交的变更，可以避免脏读，但**不可重复读和幻读**问题仍可能出现 |
| REPEATABLE READ（可重复读）      | 确保事务可以多次从一个字段中读取相同的值，在这个事务持续期间，禁止其他事务对这个字段进行更新，可以避免脏读和不可重复读，但**幻读**的问题仍然存在 |
| SERIALIZABLE（串行化）           | 确保事务可以从一个表中读取相同的行，在这个事务持续期间，禁止其他事务对该表执行插入，更新和删除操作，所有并发问题都可以避免，但性能十分低下 |

Oracle支持的2种事务级别：**READ COMMITTED**，SERIALIZABLE。Oracle默认的事务隔离级别为READ COMMITTED

Mysql支持4种事务级别，Mysql默认的事务隔离级别为**REPETABLE READ**

#### 具体演示

查看当前隔离级别

~~~sql
select @@tx_isolation;
8.0后改为select @@transaction_isolation;
show variables like 'tx_isolation';
~~~

将隔离级别设置成最低

~~~sql
set session transaction isolation level read uncommitted;
~~~

如果中文显示为乱码，更改字符集

~~~sql
set names gbk;
~~~

这时候如果开启两个事务，一个将表中姓名更改但没有提交，这时候另一个事务去读表中内容，读到了没有提交的内容，当事务1回滚，再次读的时候事务2仍读到原来的，出现脏读。

当更改事务1的隔离级别为读已提交，更改数据但不提交，更改事务2的隔离级别为读已提交，查看表中数据，无法看到表格修改的内容。因此脏读被避免。但当事务1提交后，事务2再查看表中内容，发现两次看的数据不一样，出现不可重复读。

可重复读下：

当更改事务1和2的隔离级别为可重复读，更改事务1的数据但不提交，事务2查看表中数据，为未修改前的；当事务1提交，事务2查看仍是未修改的。只有事务2提交后，再次查看才是修改后的。解决了脏读和不可重复读。但当事务1要更改所有行数据的名字，还未执行，在此时事务2插入了一条数据并commit了，这样当事务1执行的时候多影响到了事务2插入的这一条数据，有幻读出现。只有执行插入语句的事务提交后，事务1的更改语句才能执行成功。

|      | 事务A                                    | 事务B                                 |
| ---- | ---------------------------------------- | ------------------------------------- |
| T1   | SET autocommit=0;                        | SET autocommit=0;                     |
| T2   | select * from account;3条                |                                       |
| T3   |                                          | insert into account values();插入一条 |
| T4   |                                          | commit                                |
| T5   | update account set balance=100;会影响4条 |                                       |

在可重复读下，执行以上时间点的操作，可以发现，当事务A第一次读取数据，只有3条，当事务B插入一行数据后，事务A再次更新的时候，会影响4条数据，出现了幻读。但是如果此时事务A去读取数据，仍然只有3条。原因是select读是快照读，MVCC解决了快照读的幻读问题。而update为当前读，在当前读下仍然会出现幻读情况。

同时，使用select * from account for update;这种当前读的select也会出现幻读。

当事务1,2的隔离级别修改为串行化后，当事务1，2均开启，事务1要执行update操作，事务2执行插入语句，但无法执行成功，只有事务1提交后，事务2才能执行。一次只能执行一个事务。

经过试验，在最高等级下，哪个事务先被开启不重要，如果同时有两个事务被开启，事务1要执行插入操作，事务2没有执行sql语句，此时事务1的插入操作可以成功；但一旦事务2执行了sql语句，插入操作便不能成功，当插入数据的事务没有提交前，其他事务不能进行查询操作。小结：只要其他表使用了sql语句查看or操作表，修改表的操作就不能被执行；只要有其他事务修改了表，就不能查看表。看了不能改，改了不能看。

#### Mysql中设置隔离级别

- 每启动一个mysql程序，就会获得一个单独的数据库连接，每个数据库连接都有一个全局变量@@tx_isolation，表示当前的事务隔离级别

- 查看当前的隔离级别：select @@tx_isolation;

- 设置当前mysql连接的隔离级别

  set session transaction isolation level **read uncommitted**;将其替换为需要的级别

- 设置数据库的全局隔离级别

  set **global** transaction isolation level **read uncommitted**;

#### delete和truncate在事务使用时的区别

delete和truncate是针对数据的操作语言，而不是针对表结构的，要删除表使用drop table，删除数据使用delete from 表名和truncate table 表名。

演示delete

~~~sql
SET autocommit=0;
START TRANSACTION;
DELETE FROM account;
ROLLBACK;
~~~

数据并没有被删除。

演示truncate

~~~sql
SET autocommit=0;
START TRANSACTION;
TRUNCATE TABLE account;
ROLLBACK;
~~~

这时候数据仍然被删除了，没有回滚。因此truncate不支持回滚。

#### savepoint的使用

~~~sql
SET autocommit = 0;
START TRANSACTION;
DELETE FROM account WHERE id = 1;
SAVEPOINT a;#设置保存点
DELETE FROM account WHERE id = 3;
ROLLBACK TO a;#回滚至保存点
~~~

savepoint搭配rollback使用，回滚到保存点，因此id=1的行被删除，但是id=3的行没有。

### ACID的实现

> 参考博客：
>
> `https://www.cnblogs.com/kismetv/p/10331633.html`
>
> `https://blog.csdn.net/SnailMann/article/details/94724197?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task`

#### 原子性（A）的实现

原子性为一个事务是不可分割的工作单位，操作要么一起做，要么都不做，若一个sql执行失败，已经执行的语句需要回滚。

而原子性的实现依靠的是undo log（回滚日志），实现原子性的关键为可以撤销成功执行的sql语句。当事务要对数据库进行修改时，InnoDB生成对应的undo log，若事务执行失败或调用了roll back，可以使用undo log来将数据修改为回滚前的状态。

undo log是逻辑日志，记录sql执行相关信息，属于存储引擎层。当发生回滚时，InnoDB根据undo log的内容做与之前相反的操作：对insert，进行delete；对delete，进行insert；对update，执行相反的update，将数据改回去。

#### 持久性（D）的实现

持久性为事务一旦提交，对数据库的改变应该是永久性的，接下来的其他操作或故障不应该对其有影响。

持久性的实现依靠的是redo log（重做日志），与undo log回滚日志一样属于InnoDB的事务日志。

InnoDB提供缓存Buffer Pool，包含磁盘中部分数据页的映射，作为数据库的缓冲，减少IO次数。数据库读取数据的逻辑是：先在Buffer Pool中读取，若缓冲中没有，从磁盘读取后放入缓冲。当向数据库写数据，先写入缓冲，Buffer Pool中修改的数据定期刷新到磁盘中（刷脏）。Buffer Pool的优点是提高了读写效率，但问题是，若MySQL宕机，会导致Buffer Pool中数据没有刷新到磁盘，数据丢失，持久性也就无法保证。因此redo log被引入。

数据修改时，除了在Buffer Pool中修改，也会在redo log中记录；redo log默认在事务提交时刷盘，也有每秒刷盘等。MySQL宕机后，重启时可以读取redo log中的数据，对数据库进行恢复。而所有修改先写入redo log，再更新到Buffer Pool，这样可以保证持久性。

而redo log可以保证持久性的关键是它比刷脏要更快，不然刷脏很快也没有必要使用redo log，redo log更快的原因是

- 刷脏是随机IO，修改数据随机；redo log是追加操作，为顺序IO（不用多个地方移动磁头）
- 刷脏以数据页为单位，MySQL默认页是16kb，对页上做小修改要整页写入；redo log只包含真正需要写入的数据（修改量更少）

#### 隔离性（I）的实现

隔离性为事务的内部操作与其他事务隔离，并发执行的事务之间不能相互干扰。最严格的隔离性，对应事务隔离级别中的串行化。

主要考虑读与写之间的隔离性

- 事务A写操作对事务B写操作的影响：锁机制保证
- 事务写操作对事务B读操作的影响：MVCC保证

MyISAM的锁为表锁，而InnoDB为行锁，粒度更小。当事务A对一行数据执行写操作时，对相同行执行写操作的事务B会被阻塞，直到事务A提交。

并发事务会产生脏读，不可重复读和幻读问题，对应的，数据库依靠前面所说的四个隔离级别来消除这些影响，但隔离级别越高，虽然读取问题变少，但性能也变差，一般数据库默认可重复读（MySQL）或读已提高（Oracle）。

而隔离级别解决脏读、不可重复读或幻读，依靠的是MVCC，即Multi-Version Concurrency Control，多版本并发事务控制。MVCC的特点是不同事务读取到的数据可能是不同的（多版本的），因为读为快照读（读取不是最新的）。最大的优先是读不加锁，因此读写不冲突，并发性能好。InnoDB实现MVCC，多个版本的数据可以共存，主要依靠的是数据**隐藏列**（标记位），**undo log**和**Read View**（读视图）。

要实现多版本控制，需要保存有**不同版本的数据**，需要当前版本可以**找到上一个版本的数据**，和判断当前版本**可以看到哪些版本的数据**，分别依靠undo log，隐藏列与Read View。

隐藏列中包括

- 最近**修改事务ID**
- **回滚指针**：配合undo  log，指向此记录上一个版本
- 隐含的自增ID
- 实际还有删除flag隐藏字段，记录被更新或删除不是真的删除，而是flag变化

读取数据时，MySQL可以判断是否需要回滚并找到所需要的undo log，从而实现。

对MVCC有帮助的是undo log中记录了update 和 delete的日志，在快照读时有用。

当有新事务对数据进行了修改，新事务会有指针指向旧事务。

{% asset_img MVCC链表.png This is an example image %}

这样undo log形成一个单向链表，表头为最新的旧记录，表尾是最早的旧记录。

而Read View是在事务进行快照读的时候产生的读视图，维护了活跃事务的ID（自增，越新事务ID越大），用来判断当前版本的视图可以看到哪个版本的数据。其用来做数据的可见性判断，其可见性算法是，取出要修改数据的最新记录中的事务ID，与Read View中维护的活跃事务ID比较，若不符合可见性，则通过回滚指针找到undo log中上一条记录，直到找到符合可见性规则的（从表头到表尾）。

其判断的逻辑为：在Read View中维护了一系列生成时刻事务ID，其中有最小事务ID与已创建最大事务ID。

- 如果数据中当前事务ID小于生成时最小事务ID，说明此记录是在Read View前面就出现了，最新的数据是可以被看到的；若大于等于进入下一个判断
- 如果当前事务ID大于等于已创建最大事务ID，说明此记录是在Read View后才出现的，肯定对当前事务不可见；若小于，进入下一个判断
- 判断数据中事务ID是否仍在活跃事务之中，如果在，代表Read View时刻，事务还在活跃，没有commit，修改的数据当前事务也看不到；如果不在，说明事务在Read View前已经commit了，修改的结果对当前事务是可见的

如果一个数据被删除了，将版本链上的数据复制一份，更新修改事务ID，然后将标志位更改为true，如果查询到了已经删除的数据（flag为true），则不返回数据。

因此可以看出快照读的结果非常依赖于Read View生成的时机。

在读已提交下，每个快照读都会生成并获取最新的Read View，可重复读下同一个事务的第一个快照才生成Read View，之后快照读取的仍为同一个Read View。

个人小总结为：若是快照读生成的时机越早，更新的次数越少，越有可能对其他事务修改的数据不可见。

#### 一致性（C）的实现

一致性为数据库完整性约束没有被破坏，事务执行的前后数据状态都是合法的。一致性是事务的最终目标，是依靠于原子性、隔离性和持久性来实现的。在数据库层面与应用层层面都需要来保障一致性。

## 视图

含义：虚拟表，和普通表一起使用

mysql5.1版本出现的新特性，行和列的数据来自自定义视图的查询中使用的表，并且是在使用视图时**动态生成**的，**只保存了sql逻辑，不保存查询结果**

应用场景：

- 多个地方用到**同样的查询结果**
- 该查询结果使用的sql语句**较复杂**

好处：

1、简化了sql语句

2、提高了sql的重用性

3、保护了基表的数据，提高了安全性	

案例：查询姓张的学生名和专业名

以前的做法

~~~sql
SELECT stuname,majorName
FROM `stuinfo` s
INNER JOIN `major` m ON s.`majorId`=m.`id`
WHERE stuname LIKE '张%';
~~~

现在可以将常用的语句封装为视图

~~~sql
CREATE VIEW v1
AS
SELECT stuname,majorName
FROM `stuinfo` s
INNER JOIN `major` m ON s.`majorId`=m.`id`;
~~~

要调用的时候

~~~sql
SELECT * FROM v1 WHERE stuname LIKE '张%';
~~~

### 创建视图

语法：

create view 视图名

as

查询语句;

1、查询姓名中包含a字符的员工名、部门名和工种信息

~~~sql
#1、创建
CREATE VIEW myv1
AS 
SELECT last_name,department_name,job_title
FROM `employees` e
JOIN `departments` d ON e.`department_id`=d.`department_id`
JOIN`jobs` j ON j.`job_id`=e.`job_id`;
#2、使用
SELECT * FROM myv1 WHERE last_name LIKE '%a%';
~~~

2、查询各部门的平均工资级别

~~~sql
#1、创建视图查看每个部门的平均工资
CREATE VIEW myv2
AS
SELECT AVG(salary) ag,department_id
FROM employees
GROUP BY department_id;
#2、使用
SELECT myv2.`ag`,g.grade_level
FROM myv2
JOIN job_grades g
ON myv2.`ag` BETWEEN g.`lowest_sal` AND g.`highest_sal`;
~~~

3、查询平均工资最低的部门信息

~~~sql
#平均工资视图已经被创建
SELECT * FROM myv2 ORDER BY myv2.`ag` ASC LIMIT 1;
~~~

4、查询平均工资最低的部门名和工资

~~~sql
#创建平均工资最低的部门id表
CREATE VIEW myv3
AS
SELECT * FROM myv2 ORDER BY myv2.`ag` ASC LIMIT 1;
#将视图与部门表进行连接
SELECT d.*,m.ag
FROM myv3 m
JOIN departments d
ON m.`department_id`=d.`department_id`;
~~~

### 视图的好处

- 重用sql语句
- 简化复杂的sql操作，不必知道其细节
- 保护数据，提高安全性

### 视图的修改

方式一：

意思为存在就替代，不存在就创建

**create or replace** view 视图名

as

查询语句;

~~~sql
CREATE OR REPLACE VIEW myv3
AS
SELECT AVG(salary),job_id
FROM employees
GROUP BY job_id;
~~~

方式二：

语法：

alter view 视图名

as

查询语句;

~~~sql
ALTER VIEW myv3
AS
SELECT * FROM employees;
~~~

### 视图的删除

语法：

**drop view** 视图名，视图名，…;

~~~sql
DROP VIEW myv1,myv2,myv3;
~~~

### 查看视图

~~~sql
DESC myv3;
SHOW CREATE VIEW myv3;
~~~

### 视图练习

1、创建视图emp_v1，要求查询电话号码以011开头的员工姓名和工资、邮箱

~~~sql
CREATE OR REPLACE VIEW emp_v1
AS
SELECT last_name,salary,email
FROM employees
WHERE phone_number LIKE '011%';
~~~

不能先创建没有phone筛选的视图再调用视图筛选，因为视图中没有phone这一列。

2、创建视图emp_v2，要求查询的部门最高工资高于12000的部门信息

之前做法：先得到部门最高工资高于12000的工资和编号表，然后和部门表连接

~~~sql
SELECT d.*,m.mx
FROM departments d
JOIN (
	SELECT MAX(salary) mx,department_id
	FROM employees
	GROUP BY department_id
	HAVING MAX(salary)>12000
) AS m
ON d.department_id=m.department_id;
~~~

视图的做法，先将部门最高工资高于12000的工资和编号表作为视图，然后用视图和部门表连接

~~~sql
CREATE VIEW emp_v2
AS
SELECT MAX(salary) mx,department_id
FROM employees
GROUP BY department_id
HAVING MAX(salary)>12000;

SELECT d.*,emp_v2.mx
FROM departments d
JOIN emp_v2
ON d.department_id=emp_v2.department_id;
~~~

### 视图的更新

对视图的数据更新，而不是结构。一般给视图添加权限，只允许读，不允许写。

视图的可更新性和视图中查询的定义有关系，以下类型的视图是不允许更新的

- 包含以下关键字的sql语句：分组函数、distinct、group by、having、union或者union all
- 常量视图
- select中包含子查询
- join
- from一个不能更新的视图
- where子句的子查询引用了from子句中的表

~~~sql
CREATE OR REPLACE VIEW myv1
AS
SELECT last_name,email
FROM employees;
~~~

1、插入

~~~sql
INSERT INTO myv1 VALUES('张飞','zf@qq.com');
~~~

此时在原始的employees表中也插入了数据。

2、修改

~~~sql
UPDATE myv1 SET last_name='张无忌' WHERE last_name='张飞';
~~~

原始表依然被更改了

3、删除

~~~sql
DELETE FROM myv1 WHERE last_name='张无忌';
~~~

### 视图与表的比较

|      | 创建语法的关键字 | 是否实际占用屋里空间 | 使用         |
| ---- | ---------------- | -------------------- | ------------ |
| 视图 | create view      | 只是保存了sql逻辑    | 一般仅用来查 |
| 表   | create table     | 保存了数据           | 增删改查     |

## 变量

系统变量

- 全局变量

  作用域：服务器每次启动将为所有的全局变量赋初始值，针对于所有的会话（连接）有效，但不能跨重启。

- 会话变量

  作用域：仅仅针对于当前会话（连接）有效

自定义变量

- 用户变量
- 局部变量

### 系统变量

说明：变量由系统提供，不是用户定义，属于服务器层面

使用的语法	：

1、查看所有的系统变量

~~~sql
SHOW GLOBAL VARIABLES;#全局变量
SHOW 【SESSION】 VARIABLES;#会话变量
~~~

2、查看满足条件的部分系统变量

~~~sql
SHOW GLOBAL|[SESSION] VARIABLES like '%查询字符%';
~~~

3、查看指定的某个系统变量的值

~~~sql
select @@global.系统变量名;#全局变量
select @@[session.]系统变量名;#会话变量
~~~

4、为某个系统变量赋值

方式一：

~~~sql
set global|【session】 系统变量名=值;
~~~

方式二：

~~~sql
set @@global|【session】 .系统变量名=值;
~~~

注意：如果是全局级别，则需要加global，如果是会话级别，则需要加session，如果不写，则默认session

### 自定义变量

说明：变量是用户自定义的，不是由系统定义的。

使用步骤：

声明

赋值

使用（查看、比较、运算等）

1、用户变量

作用域：针对于当前的会话（连接）有效，同于会话变量的作用域

应用在任何地方，也就是begin end里面或者外面

赋值的操作符：=或:=

~~~sql
#1、声明并初始化
SET @用户变量名=值;或
SET @用户变量名:=值;或
SELECT @用户变量名:=值;
#2、赋值（更新用户变量的值）
方式一：通过SET或SELECT
    SET @用户变量名=值;或
    SET @用户变量名:=值;或
    SELECT @用户变量名:=值;
方式二：通过SELECT INTO
    SELECT 字段 INTO @变量名
    FROM 表;
#3、使用（查看用户变量的值）
    SELECT @用户变量名;	
~~~

2、局部变量

作用域：仅仅在定义它的begin end中有效

应用在 begin end中的第一句话

~~~sql
1、声明
  DECLARE 变量名 类型;
  DECLARE 变量名 类型 DEFAULT 值;
2、赋值
  方式一：通过SET或SELECT
      SET 局部变量名=值;或
      SET 局部变量名:=值;或
      SELECT @局部变量名:=值;
  方式二：通过SELECT INTO
      SELECT 字段 INTO 局部变量名
      FROM 表;
3、使用
  SELECT 局部变量名;
~~~

#### 对比用户变量和局部变量

|          | 作用域      | 定义和使用的位置                 | 语法                          |
| -------- | ----------- | -------------------------------- | ----------------------------- |
| 用户变量 | 当前会话    | 会话中的任何地方                 | 必须加@符号                   |
| 局部变量 | BEGIN END中 | 只能在BEGIN END 中，且为第一句话 | 一般不用加@符号，需要限定类型 |

案例：声明两个变量并赋初值，求和，并打印

~~~sql
#1、用户变量
SET @m = 1;
SET @n = 2;
SET @sum = @m + @n;
SELECT @sum;
#语法报错，需要使用在BEGIN END中
#2、局部变量
DECLARE m INT DEFAULT 1;
DECLARE n INT DEFAULT 2;
DECLARE SUM INT;
SET SUM=m+n;
~~~

## 存储过程和函数

类似Java中的方法

### 存储过程

#### 存储过程创建与调用

含义：一组预先编译好的SQL语句的集合，理解成批处理语句

好处：

1. 提高代码的重用性
2. 简化操作
3. 减少了编译次数并且减少了和数据库服务器的连接次数，提高了效率

一、创建语法

~~~java
create procedure 存储过程名(in|out|inout 参数名  参数类型,...)
begin
	存储过程体（一组合法的SQL语句）
end
~~~

注意：

1、参数列表包括三部分

参数模式 参数名 参数类型

举例：

IN stuname VARCHAR(20)

参数模式：

IN：该参数可以作为输入，即该参数需要调入方传入值

OUT：该参数可以作为输出，即该参数可以作为返回值

INOUT：该参数即可以作为输入又可以作为输出，该参数既需要传入值，又可以返回值

2、如果存储过程体只有一句话，begin end可以省略

存储过程中的每条sql语句的结尾必须加分号。存储过程的结尾可以使用DELIMITER重新设置

语法：

DELIMITER 结束标记

DELIMITER $

二、调用语法

CALL 存储过程名（实参列表）；

1、空参列表

案例：插入到admin表中5条记录

~~~sql
#1、创建存储过程
DELIMITER $
CREATE PROCEDURE myp1()
BEGIN
	INSERT INTO admin(username,`password`)
	VALUES('john1','0000'),('lili','0000'),('rose','0000'),('jack','0000'),('tom','0000');
END $
#2、调用
CALL myp1()$
~~~

2、创建带in模式参数的存储过程

案例1：创建存储过程实现根据女神名，查询对应的男神信息

~~~sql
#1、创建
DELIMITER $
CREATE PROCEDURE myp2(IN beautyName VARCHAR(20))
BEGIN
	SELECT bo.*
	FROM boys bo
	RIGHT JOIN beauty b
	ON bo.id = b.`boyfriend_id`
	WHERE b.name=beautyName;
END $
#2、调用
CALL myp2('小昭');
~~~

案例2：创建存储过程实现用户是否登录成功

思路：先创建存储过程来查询表中与输入信息相同的个数（count(*)），然后为了将个数赋值给变量，声明了int类型的变量result，然后用if语句来判断如果个数>0，则输出成功。

~~~sql
DELIMITER $
CREATE PROCEDURE myp4(IN username VARCHAR(20),IN PASSWORD VARCHAR(20))
BEGIN
	#声明并初始化
	DECLARE result INT DEFAULT 0;
	SELECT COUNT(*) INTO result#赋值 select into用法
	FROM admin 
	#出现冲突时，使用表名.变量名的形式
	WHERE admin.username = username
	AND admin.password = PASSWORD;
	#使用
	SELECT IF(result>0,'成功','失败');
END $

CALL myp4('张飞','8888');
~~~

3、创建带out模式的存储过程

案例1：根据女神名，返回对应的男神名

思路：查询后的结果利用select into 参数名进行传递，然后调用时可以定义用户变量在begin end外面接收，然后输出即可。

~~~sql
DELIMITER $
CREATE PROCEDURE myp5(IN beautyName VARCHAR(20),OUT boyName VARCHAR(20))
BEGIN
	SELECT bo.`boyName` INTO boyName
	FROM `boys` bo
	INNER JOIN `beauty` b
	ON bo.`id` = b.`boyfriend_id`
	WHERE b.`name`=beautyName;
END $
#调用
SET @bName;
CALL myp5('小昭',@bName);
SELECT @bName;
~~~

案例2：根据女神名，返回对应的男神名和男神魅力值

注意：为了输出多个OUT，需要在select写完后，再INTO后面加变量1，变量2。

~~~sql
DELIMITER $
CREATE PROCEDURE myp6(IN beautyName VARCHAR(20),OUT boyName VARCHAR(20),OUT userCP INT)
BEGIN
	SELECT bo.`boyName` ,bo.`userCP` INTO boyName,userCP
	FROM `boys` bo
	INNER JOIN `beauty` b
	ON bo.`id` = b.`boyfriend_id`
	WHERE b.`name`=beautyName;
END $

CALL myp6('小昭',@bName,@userCP);
SELECT @bName,@userCP;
~~~

4、创建带inout模式参数的存储过程

案例1：传入a和b两个值，最终a和b都翻倍并返回

~~~sql
DELIMITER $
CREATE PROCEDURE myp7(INOUT a INT, INOUT b INT)
BEGIN
	SET a=a*2;
	SET b=b*2;
END $

SET @m=10;
SET @n=20;
CALL myp7(@m,@n);
SELECT @m,@n;
~~~

#### 存储过程删除

语法：drop procedure 存储过程名

#### 查看存储过程的信息

show create procedure 存储过程名

#### 存储过程练习

1、创建存储过程实现传入用户名和密码，插入到admin表中

~~~sql
DELIMITER $
CREATE PROCEDURE test_pro1(IN username VARCHAR(20), IN PASSWORD VARCHAR(20))
BEGIN
	INSERT INTO admin(admin.username,admin.`password`)
	VALUES(username,PASSWORD);
END $

CALL test_pro1('admin','0000');
~~~

2、创建存储过程或函数实现传入女神编号，返回女神名称和女神电话

有复制，用select 字段 into 变量名

~~~sql
DELIMITER $
CREATE PROCEDURE test_pro2(IN beautyID INT,OUT beautyName VARCHAR(20),OUT beautyPhone VARCHAR(20))
BEGIN
	SELECT `name` ,`phone` INTO beautyName,beautyPhone
	FROM `beauty`
	WHERE id=beautyID;
END $

CALL test_pro2(12,@name,@phone);
SELECT @name,@phone;
~~~

3、创建存储过程或函数实现传入两个女神生日，返回大小

~~~sql
DELIMITER $
CREATE PROCEDURE test_pro3(IN birth1 DATETIME, IN birth2 DATETIME, OUT result INT)
BEGIN
	SELECT DATEDIFF(birth1,birth2) INTO result;
END $

CALL test_pro3('1988-1-1',NOW(),@result);
SELECT @result;
~~~

4、创建存储过程或函数传入一个日期，格式化成xx年xx月xx日并返回

~~~sql
DELIMITER $
CREATE PROCEDURE test_pro4(IN mydate DATETIME, OUT str VARCHAR(20))
BEGIN
	SELECT DATE_FORMAT(mydate,'%y年%m月%d日') INTO str;
END $

CALL test_pro4(NOW(),@str);
SELECT @str;
~~~

5、创建存储过程或函数传入女神名称，返回：女神 and 男神 格式的字符串

如：传入 小昭

返回：小昭 and 张无忌

思路：正常查询，其中字符串拼接使用CONCAT函数，中间用逗号隔开，输出存入变量，查询用右外连接，加上筛选条件。

~~~sql
DELIMITER $
CREATE PROCEDURE test_pro5(IN beautyName VARCHAR(20), OUT str VARCHAR(50))
BEGIN
	SELECT CONCAT(beautyName,' and ',boyName) INTO str
	FROM boys bo
	RIGHT JOIN beauty b
	ON bo.id=b.boyfriend_id
	WHERE b.name=beautyName;
END $

CALL test_pro5('小昭',@str);
SELECT @str;
~~~

但是这样写的话，当boyName为null的时候，拼接在一起也是null，因此最好使用ifnull来进行判断，只要有为空可能的字段，要是跟其他字符串拼接或者要使用它，最好加上IFNULL判断。

~~~sql
DELIMITER $
CREATE PROCEDURE test_pro5(IN beautyName VARCHAR(20), OUT str VARCHAR(50))
BEGIN
	SELECT CONCAT(beautyName,' and ',IFNULL(boyName,'null')) INTO str
	FROM boys bo
	RIGHT JOIN beauty b
	ON bo.id=b.boyfriend_id
	WHERE b.name=beautyName;
END $
~~~

6、创建存储过程或函数，根据传入的条目数和起始索引，查询beauty表的记录

其中用到了分页，limit 【offset】，size

~~~sql
DELIMITER $
CREATE PROCEDURE test_pro6(IN size INT, IN startIndex INT)
BEGIN
	SELECT * FROM beauty LIMIT startIndex,size;
END $

CALL test_pro6(3,5);
~~~

### 函数

与存储过程的区别是：

- 存储过程：可以有0个返回，也可以有多个返回，适合做批量插入、批量更新
- 函数：有且仅有1个返回，适合做处理数据后返回一个结果

#### 函数创建

1、创建语法

create function 函数名（参数列表）**returns** 返回类型

begin

​	函数体

end

注意：

- 参数列表包含两部分，参数名  参数类型

- 函数体：肯定会有return语句，如果没有会报错，如果return语句不放在函数体最后不报错，但不建议
- 函数体中仅有一句话，可以省略begin end
- 使用delimiter语句设置结束标记

#### 函数调用

执行语句并显示返回值

语法：

select 函数名（参数列表）

1、无参有返回

案例：返回公司的员工个数

~~~sql
DELIMITER $
CREATE FUNCTION myf1() RETURNS INT
BEGIN
	DECLARE c INT DEFAULT 0;#定义变量
	SELECT COUNT(*) INTO c#赋值
	FROM employees;
	RETURN c;
END $
#调用
SELECT myf1();
~~~

2、有参有返回

案例1：根据员工名，返回它的工资

~~~sql
DELIMITER $
CREATE FUNCTION myf2(empName VARCHAR(20)) RETURNS DOUBLE
BEGIN
	SET @sal=0;#定义用户变量
	SELECT salary INTO @sal#赋值
	FROM employees
	WHERE last_name = empName;
	RETURN @sal;#返回值
END $

SELECT myf2('Kochhar');
~~~

案例2：根据部门名，返回该部门的平均工资

规律：

- 记得写returns 变量 类型
- 在begin end中定义变量（declare sal double）or （set @sal double）
- 利用select 字段 into 变量赋值
- 最后return 变量。

~~~sql
DELIMITER $
CREATE FUNCTION myf3(depName VARCHAR(20)) RETURNS DOUBLE
BEGIN
	#定义返回值
	DECLARE sal DOUBLE;
	SELECT AVG(salary) INTO sal
	FROM employees e
	JOIN departments d ON e.`department_id`=d.`department_id`
	WHERE d.`department_name`= depName;
	RETURN sal;
END $

SELECT myf3('IT');
~~~

#### 查看函数

语法

show create function 函数名;

#### 删除函数

语法

drop function 函数名；

#### 函数练习

案例：创建函数，实现传入两个float，返回二者之和

这里给变量赋值，可以使用set 变量=语句，也可以使用select 语句 into 变量名

~~~sql
DELIMITER $
CREATE FUNCTION myf4(num1 FLOAT,num2 FLOAT) RETURNS FLOAT
BEGIN
	DECLARE num3 FLOAT;
	SET num3 = num1 + num2;
	RETURN num3;
END $

SELECT myf4(1,2);
~~~

## 流程控制结构

- 顺序结构：程序从上往下一次执行
- 分支结构：程序从两条或多条路径中选择一条去执行
- 循环结构：程序在满足一定条件的基础上，重复执行一段代码

### 分支结构

#### IF函数

功能：实现简单的双分支

select If(表达式1，表达式2，表达式3)

执行顺序：如果IF函数成立，则IF函数返回表达式2的值，否则返回表达式3的值

应用在任何地方

#### CASE结构

情况1：类似于java中的switch语句，一般用于实现等值判断

语法：

case 变量|表达式|字段

when 要判断的值 then 返回的值1或语句1【；】

when 要判断的值 then 返回的值2或语句2【；】

…

else 要返回的值 n或语句n【；】

end 【case;】

情况2：类似于java中的多重IF语句，一般用于实现区间判断

case 

when 要判断的条件1 then 返回的值1或语句1【；】

when 要判断的条件2 then 返回的值2或语句2【；】

…

else 要返回的值 n或语句n【；】

end 【case；】

特点：

1. 可以作为表达式，嵌套在其他语句中使用，可以放在任何地方，begin end中或外面

   可以作为**独立的语句**使用，只能放在begin end中

2. 如果when中的值满足或条件成立，则执行对应的then后面的语句，并且结束case 

   如果都不满足，则执行else中的语句或值

3. else可以省略，如果else省略了，并且所有的when条件都不满足，则返回null

案例：创建存储过程，根据传入的成绩来显示等级，如传入成绩：90-100，显示A；80-90，显示B；60-80，显示C；否则显示D

~~~sql
DELIMITER $
CREATE PROCEDURE test_case(IN score INT)
BEGIN
	CASE 
	WHEN score>=90 AND score<=100 THEN SELECT 'A';
	WHEN score>=80 THEN SELECT 'B';
	WHEN score>=60 THEN SELECT 'C';
	ELSE SELECT 'D';
	END CASE;
END $

CALL test_case(55);
~~~

#### IF结构

功能：实现多重分支

语法：

if 条件1 then 语句1；

elseif 条件2 then 语句2；

…

【else 语句n;】

end if;

应用场景：应用在begin end中

案例：根据传入的成绩，来显示等级，比如传入成绩：90-100，返回A；80-90，返回B；60-80，返回C；否则返回D

~~~sql
DELIMITER $	 
CREATE FUNCTION test_if(score INT) RETURNS CHAR
BEGIN
	IF score>=90 AND score<=100 THEN RETURN 'A';
	ELSEIF score>=80 THEN RETURN 'B';
	ELSEIF score>=60 THEN RETURN 'C';
	ELSE RETURN 'D';
	END IF;
END $

SELECT test_if(86);
~~~

### 循环结构

分类：while、loop、repeat

循环控制：

iterate类似于continue，继续，结束本次循环，继续下一次

leave类似于break，跳出，结束当前所在的循环

#### while

语法：

【标签：】while 循环条件 do

​	循环体;

end while【标签】；

#### loop

语法：

【标签：】loop

​	循环体；

end loop【标签】；

可以用来模拟简单的死循环

#### repeat

类似于do while，至少执行一次

语法：

【标签：】repeat

​	循环体；

until 结束循环的条件

end repeat 【标签】；

#### 循环的使用

没有添加循环控制语句

案例：批量插入，根据次数插入到admin表中多条记录

~~~sql
DELIMITER $
CREATE PROCEDURE pro_while1(IN insertCount INT)
BEGIN
	#定义循环变量i，int类型初始值为1
	DECLARE i INT DEFAULT 1;
	WHILE i<=insertCount DO
		INSERT INTO admin(username,`password`) VALUES(CONCAT('rose',i),'666');
		SET i=i+1;
	END WHILE; 
END $

CALL pro_while1(10);
~~~

添加leave语句

案例：批量插入，根据次数插入到admin表中多条记录，如果次数>20则停止

这里配合了IF结构来使用

~~~sql
DELIMITER $
CREATE PROCEDURE test_while1(IN insertCount INT)
BEGIN
	DECLARE i INT DEFAULT 1;
	a:WHILE i<=insertCount DO
		INSERT INTO admin(username,`password`) VALUES(CONCAT('hua',i),'0000');
		IF i>=20 THEN LEAVE a;
		END IF;
		SET i=i+1;
	END WHILE a;	
END $

CALL test_while1(30);
~~~

添加iterate语句

案例：批量插入，根据次数插入到admin表中多条记录，只插入偶数次

思路：在不为偶数次的时候，iterate退出即可。

~~~sql
DELIMITER $
CREATE PROCEDURE test_while2(IN insertCount INT)
BEGIN
	DECLARE i INT DEFAULT 0;
	a:WHILE i<=insertCount DO
		SET i=i+1;
		IF MOD(i,2)!=0 THEN ITERATE a;
		END IF;
		INSERT INTO admin(username,`password`) VALUES(CONCAT('xi',i),'0000');
	END WHILE a;	
END $

CALL test_while2(20);
~~~

#### 循环总结

{% asset_img 循环.png This is an example image %}

## 重要函数

### IFNULL【重要】

IFNULL（变量名，常量）

如果变量为空，则用常量代替。

只要有为空可能的字段，要是跟其他字符串拼接或者要使用它，最好加上IFNULL判断。

### SET NAMES gbk;

要是字符集报错，一般需要重新设置字符集

### DATEDIFF（time1,time2）

返回两个datetime类型的时间差，返回为整数

### NOW()

返回现在的时间，datetime类型

### CONCAT()

拼接字符串，中间用逗号隔开

### LIMIT

limit 【offset】，size

用来分页，其中offset为开始的地方，从0开始，size表示数据量。

## 牛客网题目	

1、查找当前薪水(to_date='9999-01-01')排名第二多的员工编号emp_no、薪水salary、last_name以及first_name，不准使用order by

思路：不能使用order by，就只能先找到最高薪水，然后将其排除在salary的选择之中（NOT IN），然后在剩下的工资中选择最大的，即为总体第二大的，用到的是内连接+where子查询的嵌套。

~~~sql
select e.emp_no,max(salary) salary ,e.last_name,e.first_name
from employees e
join salaries s
on e.emp_no=s.emp_no
where s.to_date='9999-01-01'
and s.salary not in (
    select max(salary)
    from salaries
    where to_date='9999-01-01'
)
~~~

2、查找员工编号emp_no为10001其自入职以来的薪水salary涨幅值growth

~~~sql
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));
~~~

将工资按照时间排序，找出第一个和最后一个，然后二者相减。考察的是**select的子句查询**。最外层的select不用加from表，因为用不到。

~~~sql
select 
(select salary from salaries where emp_no = 10001 order by from_date desc limit 1)
-(select salary from salaries where emp_no = 10001 order by from_date asc limit 1)
as growth;
~~~

## JDBC

Java DtaBase Connectivity

将Java程序与数据库连接在一起。JDBC统一接口，java程序与JDBC通信，JDBC通过不同驱动连接不同的数据库，相当于在java和数据库之间加了一层。一层不行就加两层。

### Driver Manager

就是一个类库。通过Maneger连接不同的数据库

### JDBC使用

1. 加载对应数据库的驱动
2. 根据指定的URL建立连接
3. 通过连接过去语句对象
4. 在语句对象中写入要执行的语句，获取结果集
5. 遍历结果集，获取数据
6. 关闭资源

~~~java
        //1、加载驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
        //2、通过Manager创建连接
        String url = "jdbc:mysql://localhost/girls?user=root&password=密码&serverTimezone=UTC";
        Connection conn = DriverManager.getConnection(url);
        //3、创建语句对象
        Statement statement = conn.createStatement();
        //4、用语句对象去执行语句
        ResultSet resultSet = statement.executeQuery("select * from admin");
        //5、获取结果集中内容
        while (resultSet.next()) {
            System.out.println(resultSet.getInt("id"));
        }
        //6、关闭资源
        resultSet.close();
        statement.close();
        conn.close();
~~~

使用技巧：语句对象中执行的sql语句先在客户端执行，看是否正确；结果集的指针指向第一条记录的上面，因此next后才是第一条语句。

完善的写法如下，在加载的时候，进行try,catch，在创建的时候，进行try，catch，在关闭的时候，进行判断是否为空，然后在finally中关闭。

~~~java
public static void main(String[] args) {
        try {
            //1、加载驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        //2、通过Manager创建连接
        String url = "jdbc:mysql://localhost/girls?user=root&password=qkxzs&serverTimezone=UTC";
        Connection conn = null;
        //3、创建语句对象
        Statement statement = null;
        //4、用语句对象去执行语句
        ResultSet resultSet = null;
        try {
            conn = DriverManager.getConnection(url);
            statement = conn.createStatement();
            resultSet = statement.executeQuery("select * from admin");
            //5、获取结果集中内容
            while (resultSet.next()) {
                System.out.println(resultSet.getInt("id"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            //6、关闭资源
            try {
                if (resultSet != null)
                    resultSet.close();
                if (statement != null)
                    statement.close();
                if (conn != null)
                    conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
~~~

如果是传入的参数是不确定的，如sql语句中大于某个字段的值一开始不确定，那么sql语句中用占位符?先表示，然后使用PreparedStatement，将占位符？进行替换，再进行查询。

~~~java
		//3、创建语句对象
        PreparedStatement ps = null;
        //4、用语句对象去执行语句
        ResultSet resultSet = null;
        int id = 10;
        String sql = "select * from admin where id > ?";
        try {
            conn = DriverManager.getConnection(url);
            ps = conn.prepareStatement(sql);
            ps.setInt(1,id);//替换在第一个问号处
            resultSet = ps.executeQuery();
            //5、获取结果集中内容
            while (resultSet.next()) {
                System.out.println(resultSet.getInt("id")+" "+resultSet.getString("username"));
            }
        }
~~~

如果是sql语句为insert，update，delete这种更新的语句，则不需要获取结果集，而且执行语句时使用statement.executeUpdate即可。

~~~java
Statement statement = null;
        String sql = "insert into admin values(64,'jdbc','0001')";
        //String sql = "update admin set username='jdbc2' where id = 64";
		//String sql = "delete from admin where id = 64";
        try {
            conn = DriverManager.getConnection(url);
            statement = conn.createStatement();
            statement.executeUpdate(sql);//insert update delete
        }
~~~

如果是更新的数据不确定，同样使用PreparedStatement即可。

~~~java
		//3、创建语句对象
        PreparedStatement ps = null;
        String sql = "insert into admin values(?,?,?)";
        try {
            conn = DriverManager.getConnection(url);
            ps = conn.prepareStatement(sql);
            ps.setInt(1,64);
            ps.setString(2,"jdbc3");
            ps.setString(3,"0002");
            ps.executeUpdate();//insert update delete
        }
~~~

### 事务

步骤：

1、connection关闭自动提交

2、一组sql语句

3、connection提交

4、在catch中，有异常时回滚

~~~java
public static void main(String[] args) {
        try {
            //1、加载驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        //2、通过Manager创建连接
        String url = "jdbc:mysql://localhost/test?user=root&password=qkxzs&serverTimezone=UTC";
        Connection conn = null;
        //3、创建语句对象
        Statement statement = null;
        try {
            conn = DriverManager.getConnection(url);
            statement = conn.createStatement();
            conn.setAutoCommit(false);//关闭自动提交
            statement.executeUpdate("update account set balance= 50 where id = 1");
            statement.executeUpdate("update account set balance= 150 where id = 2");
            conn.commit();//手动提交
        } catch (SQLException e) {
            e.printStackTrace();
            try {
                conn.rollback();//有异常时回滚
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
        } finally {
            //6、关闭资源
            try {
                if (statement != null)
                    statement.close();
                if (conn != null)
                    conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
~~~

## 快捷键

### MySQL客户端

- F12 将选中语句自动换行

# MySQL高级

## 数据库范式

有点类似设计模式中设计原则，即如何设计设计模式，而数据库范式就是如何设计数据库，即设计数据库需要满足的规范。

有第一范式、第二范式、第三范式、BC范式、第四范式和第五范式

范式并不是越高越好，范式越高，数据库冗余越小，查询性能越低。一般满足第三范式即可。

### 第一范式

数据库的每一列是原子性的，不可再分。即一个列中不应该还能再拆分为多个属性。

### 第二范式

要满足第一范式，属性完全依赖于主键，不能部分依赖。如果有2个主键，有个属性只依赖于其中一个主键，就不满足第二范式。

### 第三范式

要满足第二范式，属性不能依赖于其他非主键属性，只依赖于主键。

简单来说，就是拆表。

## 逻辑架构

1. 连接层

   客户端与连接服务，主要完成类似于连接处理、授权认证及相关的安全方案。该层上引入了线程池的概念，为通过认证安全接入的客户端提供线程。可实现基于SSL的安全链接。

2. 服务层

   完成大多数的核心服务功能，如SQL接口，完成缓存的查询，SQL的分析和优化及部分内置函数执行。

3. 引擎层

   存储引擎层，存储引擎真正负责MySQL中数据的存储和提取，服务器通过API与存储引擎通信。

4. 存储层

   数据存储层，将数据存储在运行与裸设备的文件系统之上，并完成与存储引擎的交互

### 存储引擎

插件式存储引擎架构将查询处理和其他的系统任务以及数据的存储提取相分离。

存储引擎是针对表而生效的，而不是数据库。

查看存储引擎

show engines;

show variables like ‘%storage_engine%’;

MySQL默认InnoDB

#### MyISAM与InnoDB

| 对比     | MyISAM                                             | InnoDB                                                       |
| -------- | :------------------------------------------------- | ------------------------------------------------------------ |
| 主外键   | 不支持                                             | 支持                                                         |
| 事务     | 不支持                                             | 支持                                                         |
| 行表锁   | 表锁，操作一条数据锁整个表，不适合高并发           | 行锁，操作只锁一行，不对其他行有影响。适合**高并发**         |
| 缓存     | 只缓存索引，不缓存真实数据                         | 缓存索引与真实数据，对内存要求高，而且内存大小对性能有决定性影响 |
| 表空间   | 小                                                 | 大                                                           |
| 关注点   | **性能**                                           | **事务**                                                     |
| 默认安装 | 是                                                 | 是                                                           |
| 表文件   | 3个，frm存放表结构信息，MYD存放数据，MYI存放表索引 | 2个，frm存放表结构信息，                                     |

阿里巴巴大部分mysql数据库使用的为percona的原型加以修改，percona提升了高负载下InnoDB性能，更多的诊断与控制行为。

## 索引

性能下降SQL慢，如果执行时间长或者等待时间长

- 查询语句写的烂
- 索引失效
- 关联查询太多join（设计缺陷或不得已的需求）
- 服务器调优及各个参数设置（缓冲、线程数等）

{% asset_img sql顺序.png This is an example image %}

sql的执行顺序为

1. from
2. join
3. where
4. group by
5. having
6. select
7. order by
8. limit

### join分类

{% asset_img 左外右外全外.png This is an example image %}

{% asset_img 三种连接.png This is an example image %}

在左外，右外，全连接上加筛选得到。

### 索引的认识

索引Index是帮助MySQL高效获取数据的数据结构，即索引的本质是**数据结构**。	

索引的目的是提高查询效率，可以类比字典。直观理解为**排好序**的**快速查找**数据结构。影响where的查找与order后的排序。

数据库分别维护着索引与数据，索引指向数据。

二叉查找树，左子树所有节点值比当前值小，右子树所有节点值比当前值大，左右子树分别为二叉查找树。为了检验，可以用中序遍历，得到为递增序列就是二叉查找树。这样用二叉查找树，每次可以过滤一半数据，而树的节点指向了数据。

因为索引本身也很大，一般以索引**文件**的形式存在于**磁盘**上。 （索引存在于文件系统中）

索引的文件存储形式与**存储引擎**有关（InnoDB将索引与数据存在一起，MyISAM将索引与数据分开存放）

放在磁盘中，就会存在IO，需要减少IO的时间

- 减少IO的量
- 减少IO的次数

平常说的索引，一般默认指B树（多路搜索树，不一定为二叉）结构组织的索引。

### 索引的优势与劣势

优势

- 提高数据**检索的效率**，降低数据库的IO成本
- 通过索引对数据**排序**，降低数据排序的成本，降低CPU消耗

劣势

- 索引列占据空间
- 索引虽然提高了查询速度，但会降低更新表的速度，因为索引需要更新
- 索引只是提高效率的一个因素，还需要优化索引建立或优化查询

### MySQL索引分类

1、单值索引

一个索引只包含单个列，一个表可以有多个单列索引

2、唯一索引

索引值必须要唯一，但允许有空值

3、复合索引

一个索引包含多个列

### 密集索引与稀疏索引

- 密集索引文件中每个搜索码值对应一个索引值
- 稀疏索引只为索引码的某些值建立索引项（需要找到对应地址，再继续搜索）

{% asset_img 密集索引.png This is an example image %}

而下图展示了InnoDB和MyISAM的索引图

{% asset_img 密集索引2.png This is an example image %}

对于Innodb

- 主键为密集索引，叶子节点存储了索引和行数据，主键检索可以直接查找
- 对于非主键索引，其为稀疏索引，需要先在稀疏索引的B+树中检索定位主键信息，在主键中进行第二次寻找，在叶子节点中获取数据

对MyISAM

- 主键索引与非主键索引均为稀疏索引，即索引的叶子节点中不存储数据，索引与数据分开存储
- 对表来说这两个索引没有区别，非主键索引不用访问主键索引来获取数据

### 索引语法

#### 创建

create [unique] index indexName on mytable(columnname(length));

alter table mytable add [unique] index [indexName] on (columnname(length));

#### 删除

drop index [indexName] on mytable;

#### 查看

show index from table_name\G

#### 详细添加索引

alter table tbl_name add primary key(column_list)：添加一个主键，以为索引必须唯一，且不能为空

alter table tbl_name add unique index_name(column_list)：创建索引值必须唯一（除了null，null可能出现多次）

alter table tbl_name add index index_name (column_list)：添加普通索引，索引值可出现多次

alter table tbl_name add fulltext index_name (column_list)：该语句指定了索引为fulltext，用于全文索引

### MySQL索引结构

1、BTree索引

B树与B+树。

对B+树，**真实数据只存在于叶子节点**，**非叶子节点不存储真实的数据**，只存储指引搜索方向的数据项。树中每一层都是不同的磁盘块，读取算一次IO，树的高度越高，IO次数越多。

2、Hash索引

只有memory才显式支持hash

InnoDB支持哈希索引，自适应的，人为不可控制

3、full-text全文索引

4、R-Tree索引

#### 为什么用B+树而不用红黑树

常见的树

BST：Binary Search Tree，二分查找树。左子树小，右子树大，极端情况下会退换成链表。

AVL：二叉平衡查找树，二叉树，节点有大小要求，左右子树高度差不超过1，让树更平衡。用低的插入性能换高的查询性能。适用于查询更多，但若插入更多，不适合。

红黑树：也是二叉平衡查找树，最长路径不超过最短路径2倍即可。数据多时，树的高度会比较高。

红黑树的旋转涉及到整棵树，因此需要将全部数据加载进内存，而且树的高度较高，而索引结构通常很大，一般以索引文件存在于硬盘中，使用红黑树IO次数多，因此红黑树不太适合索引的设计，而B树（B与B+）分块从磁盘中读取文件，只读取需要的块，树的高度更低，IO次数更少，适用于外部数据的读取，更适合索引的结构设计。

B+树结点数据存储一般应该为页的整数倍，Mysql在读取时候，默认一页为16k。

#### B树与B+树区别

> 详细的数据结构，可参考此网站，动态展示了树的变化过程
>
> `https://www.cs.usfca.edu/~galles/visualization/Algorithms.html`

B树与B+树均为平衡多路查找树，避免了二叉查找树树高度过高IO频繁的问题，所有叶节点高度相同，因此树是平衡的。

B树在所有节点上既存储数据又存储索引。如果一个节点为16k，而因为数据放在每一个节点中，数据需要占据一定的内存，因此每个节点存放数据大小有限。

而B+树在B树的基础之上，仅用叶子节点存储数据或指向数据的指针（InnoDB中主键索引中叶子节点有数据，非主键索引存放主键索引地址；MyISAM中主键与非主键索引均存放数据地址），非叶子节点的关键字个数与指针相同，可存储更多关键字，非叶子节点不存放数据，是从叶子节点中提取出的冗余索引，便于去查找数据，所有节点索引从左至右递增。所有的**叶子节点**有指针指向下一个节点。

{% asset_img B+树节点.png This is an example image %}

每个节点如果设置太大，那么单次IO时间太长；如果设置太小，则树的高度不可控。Mysql默认设置一个节点16kb（一页），如果索引设置bigint为8字节，存储地址的字符大小为6字节，这样单个索引+指针占用14字节，这样可以存储1170个索引。若叶子节点存放数据，单个索引+数据大小假设为16kb，则一个叶子节点可放16个索引+数据，则总的为1170x1170x16约等于2.2kw。因此B+树可以存储更多数据，一般3层可以支持千万级别的数据量查询。而根节点一般常驻内存，3层的B+树一次查询的IO次数为2。

这样，B+树的优势为：更有利于对数据库的**扫描**与**范围查询**（叶子节点存在指针）。

B树的优势为：在某些查询条件下，查询效率更高，为O(1)。

数据结构没有好坏之分，只有合不合适。

因此，对于Mysql这种关系型数据库，表与表之间的关联性很强，利用一个表的字段去遍历另一个表的数据是非常常见的操作，因此使用B+树方便去遍历与范围查找，比较符合关系型数据库的应用场景。

而对于MongoDB，其为NoSQL数据库的一种，文档型数据库，用类似JSON的方式存储数据，用键的形式去获取数据效率较高，数据量很大而且注重效率。B树的key与data有对应关系，且单次查询效率更高，因此可以使用B树更符合这种非关系型数据库的应用场景。

而hash索引的效率理论上要更高，hash索引只支持IN或者=的查询，无法支持范围查询，因此在等值查询较少，范围查询更多的情况下，hash索引不太适合。hash表中需要将数据放入内存，适合memory。

而B*树在B+树的基础之上，非叶子节点之间也存在指针。

#### 回表

InnoDB与MyISAM索引均为红黑树，但前者为聚集索引和密集索引，后者为非聚集索引和稀疏索引。InnoDB为聚集索引，将索引与数据存放在一起，主键索引可通过叶子节点访问数据，而非主键索引需要先找到对应主键再获取数据（需要2棵B+树查找），因此数据量较少的时候建立索引效率不一定会更高，这就是**回表**。

对比以下两个语句

~~~sql
select id from 表名 where name="zs";
select * from 表名 where name="zs";
~~~

如果对id为主键索引，name也建立了索引，第一条语句在name索引的B+树叶子节点可以直接找到需要的id，而第二条数据，就必须用name索引树中获取的id在主键索引中回表，再次查找获取数据。不需要经过回表过程的叫做索引覆盖（查询的字段与建立索引相同）

而MyISAM索引与数据分开，为非聚集索引，叶子节点上存储的指向数据的指针，非主键索引不需要先找到主键索引再获取数据。

此聚集索引，如果在指定了主键情况下，为主键；如果有非空唯一键，为唯一键；如果均为没有，会自动生成一个隐藏的rowID作为聚集索引。

### 建立索引情况

#### 建立索引情况

1. **主键**自动建立唯一索引
2. **频繁**作为**查询**条件的字段应该创建索引
3. 查询中与其他表相关联的字段，**外键**关系创立索引
4. 在高并发下倾向于创建**组合索引**
5. 查询中**排序**的字段，排序字段若通过索引访问将大大提高排序速度
6. 查询中统计或者分组字段

#### 不建立索引情况

1. **where条件里用不到**的字段不要创建索引

2. 表**记录太少**

3. 经常增删改的表

4. 若某个数据列包括太多重复内容，建立索引无太大效果

   数据重复且平均分配的值建索引效果不大

### 性能分析

#### Mysql Query Optimizer

Mysql自带的分析工具，负责SELECT语句优化器，主要功能，提供Mysql认为最优的数据检索方式，但不认为DBA认为是最优的

#### Mysql常见瓶颈

- CPU：CPU在饱和的时候一般发生在数据装入内存或从磁盘读取数据的时候
- IO：磁盘I/O瓶颈发生在装入数据远大于内容容量的时候
- 服务器硬件性能瓶颈：top，free，iostat与vmstat来查看系统性能状态

#### explain

是什么？（查看执行计划）

使用explain关键字可以模拟优化器执行SQL查询语句，从而知道Mysql如何处理你的SQL语句，分析你的查询语句或是表结构的性能瓶颈。

能干嘛 ？

- 表的读取顺序：id
- 数据读取操作的操作类型：select_type
- 哪些索引可以使用
- 哪些索引被实际使用
- 表之间的引用
- 每张表有多少行被优化器查询

怎么用？

**explain+SQL语句**

表头信息

id|select_type|table|type|possible_keys|key|key_len|ref|rows|Extra

**id**

select查询的序列号，包含一组数字，表示查询中执行的select子句或操作表的顺序

三种情况

- id**相同**，**执行顺序由上至下**
- id**不同**，如果是子查询，id的序号会递增，**id值越大优先级越高**
- id不同，同时存在（大的先行，相同的从上至下）

衍生=derived

**select_type**

1. simple：简单的select查询，不包括子查询或者union
2. primary：查询中若包含复杂的子部分，最外层查询被标记
3. subquery：在select或where列表包含了子查询
4. derived：在from列表中包含的子查询被标记为derived（衍生），Mysql会递归执行这些子查询，将结果放在**临时表**中
5. union：若第二个select出现在union之后，被标记为union；若union包含在from子句的子查询中，外层select将被标记为derived
6. union result：从union表获取结果的select

table：这行数据关于哪张表

**type**

访问类型排列

显示**查询**使用了何种**类型**，从最好到最差以此是

system>const>eq_ref>ref>range>index>All

一般得保证查询至少达到**range**级别，**最好能到ref**

system：表只有一行记录（等于系统表），为const类型的特例，平时不会出现

const：通过索引一次就找到了，const用于比较primary_key或者unique索引。因为只匹配一行数据，所以很快。如将索引置于where列表中，Mysql就能将该查询优化为常量

eq_ref：唯一性**索引扫描**，对于每个索引键，**表中只有一条记录与之匹配**，常见于主键或唯一索引扫描

ref：**非唯一性索引扫描**，返回匹配某个单独值的所有行。本质上也是一种**索引访问**，它返回所有匹配某个单独值的行，然而，可能会找到**多个符合条件的行**，应属于查找与扫描的混合体。

range：只检索给定范围的行，使用一个索引来选择行。一般为出现了between，and，<，>，in等的查询

index：full index scan，index与all区别为index类型只遍历索引树，通常比all快。（index从索引中读，all从硬盘中读）

all：全表扫描

**possible_keys与key**

- 是否使用到索引
- 索引竞争时，最后用到哪个索引

possible_key：显示可能应用在这个表上的索引，**不一定被实际使用**

key：实际使用的索引，如果没null，没有使用到索引。查询中若使用了**覆盖索引**，该索引仅出现在key列表中。（**查询的字段与建立的复合索引匹配**）

**key_len**

表示索引中使用的字节数，可通过该列计算查询中使用的索引长度。在不损失精度下，越短越好。索引字段的最大可能长度，**并非实际使用长度**。根据表定义计算而得，不是通过表内检索出的。

**ref**

 显示索引的哪一列被使用，如果可能的话，是一个常数。哪些列或常量被用于查找索引列上的值。

**rows**

根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数（越小越好） 

**Extra**

包含不适合在其他列中显示但十分重要的信息

- **Using filesort**：说明mysql会对数据使用一个外部的索引排序，而不是按照表内的索引顺序读取。Mysql无法使用索引完成的排序操作称为 “文件排序”。出现了**不好**！如索引顺序为col1，col2，col3，排序时从col3开始

- **Using temporary**：使用了临时表保存中间结果，Mysql在对查询结果排序时使用临时表。常见于排序order by和分组查询group by。出现了**很不好**！**group by的个数和顺序最好和索引相同**！

- **Using index**：相应的select操作中使用了覆盖索引，避免访问了表的数据行，**效率不错**！如果同时出现using where，说明索引被用来执行索引键值的查找；如果没有，说明索引用来读取数据而非执行查找动作。

  覆盖索引：建立了索引，且查询的列与索引的顺序与个数正好匹配。如果要使用覆盖索引，select列表中只取出所需要的列，不可select *。

- Using where：使用了where
- using join buffer：使用了连接缓存
- impossible where：where子句的值为false
- select tables optimized away：优化索引操作
- distinct：优化distinct操作

### 索引优化

#### 索引分析

##### 单表

先检验是否能查询出结果，再看如何优化

~~~sql
CREATE TABLE IF NOT EXISTS `article`(
	id INT(10) NOT NULL PRIMARY KEY AUTO_INCREMENT,
	author_id INT(10) NOT NULL,
	category_id INT(10) NOT NULL,
	views INT(10) NOT NULL,
	comments INT(10) NOT NULL,
	title VARBINARY(255) NOT NULL,
	content TEXT NOT NULL	
);

INSERT INTO article(`author_id`,`category_id`,`views`,`comments`,`title`,`content`) 
VALUES(1,1,1,1,'1','1'),(2,2,2,2,'2','2'),(1,1,3,3,'3','3');
~~~

查询的为category_id为1，评论数>1，views最多的作者id

~~~sql
EXPLAIN SELECT id,`author_id` FROM `article` WHERE `category_id`=1 AND `comments`>1 ORDER BY views DESC LIMIT 1;
~~~

查询结果为

{% asset_img 单表优化1.png This is an example image %}

可以看出type为最差的All即全表扫描，Extra中有Using filesort，最坏的情况，需要优化。

因此需要建立索引，在where后面出现的可以建立索引。

尝试1：给where和order by后的`category_id，comments，views建立符合索引

~~~sql
CREATE INDEX idx_article_ccv ON article(category_id,comments,views);
~~~

{% asset_img 单表优化2.png This is an example image %}

这时候type为range，不是全表扫描了，但是Using filesort。因为是comment>1，范围会导致索引失效，Mysql无法利用索引对views部分进行检索，后面的索引用不上。如果此时comment为=1，便可以用上。

将当前索引删除

~~~sql
DROP INDEX idx_article_ccv ON article;
~~~

尝试2：不将comment作为索引

~~~sql
CREATE INDEX idx_article_cv ON article(category_id,views);
~~~

这时候，再次查看explain结果

{% asset_img 单表优化3.png This is an example image %}

发现type为ref，用到了索引，而且没有全文扫描，达到效果。

##### 两表

两表连接时，当两个join时，索引该如何建立呢？

当有两个表，使用left join的时候，连接条件为，发现两个表的type都为all，因此可以建立索引。

左连接，索引加在右表，右表此时为ref，使用到了索引。

左连接，索引加载左表，左表为index，效果没有加在右表上好。

因为左连接的特性：左边一定有。left join条件用于确定如何从右边搜索行，左边一定都有。**右边为关键点，一定要建立索引**。

因此，左连接，给右表建立索引；右连接，给左表建立索引。换而言之，有索引的表作为从表。

join语句的优化：

- 尽可能减少join语句中的循环次数，**用小表驱动大表**。
- 优先优化NestedLoop的内层循环
- 保证join语句中被驱动表上join条件字段已经被索引
- 当无法保证被驱动表的join字段被索引且内存资源充足的情况下，不要太吝惜JoinBuffer的设置

#### 索引失效

1. 全值匹配最好（where查询为全部索引）

2. **最佳左前缀法则**（火车头不能丢，中间车厢不能断）

   如果索引了多列，要遵循最佳左前缀法则，查询从索引的**最左前列开始**并且**不跳过索引中的列**

   如果索引顺序为i1，i2，i3，查询时候为i3，i2，i1，会被Mysql**优化**，仍然可以用复合索引查询

3. **不要在索引列上做任何操作**（计算、函数、（自动或手动）**类型转换**）（**索引操作就失效**）

4. 存储引擎不能使用索引中范围条件右边的列（**范围之后全失效**！）

5. 尽量使用覆盖索引（只访问索引的查询，索引列和查询列一致），减少select *

6. mysql在使用**不等于**（!=或<>）的时候无法使用索引会导致全表扫描（少用不等）

7. is null,is not null也无法使用索引

8. like以**通配符开头**(%abc)，mysql索引失效（最好只在右边写%）

   如果需要用到左边%，用**覆盖索引**避免全表扫描（查询字段为索引中的一些或全部，不能超过）

9. 字符串不加单引号索引失效（隐式类型转换）

10. 用or连接时**索引失效**，**少用or**

如果order by中用到的字段与前面用于筛选的不连续，会有filesort。核心就在于索引是否能被连贯的使用到。

如索引为c1,c2,c3,c4

查询为where c1 order by c3,c2会有filesort

查询为where c1 c2 order by c3,c2不会有，因为c2被筛选了是连贯的

定值是常量，范围之后是失效，最终看排序，一般order by给个范围

group by基本需要排序，会有临时表产生

一般建议

- 对于单键索引，尽量选择针对当前query过滤性更好的索引
- 选择组合索引时，当前Query过滤性最好的字段在索引字段顺序中，位置越靠前越好
- 选择组合索引时，尽量选择可以能够包含当前query中的where字句中更多字段的索引
- 尽可能通过分析统计信息和调整query的写法来达到选择合适索引的目的

最好的是全值匹配，要遵守最左前缀；索引上不要计算，范围之后会失效，like 百分放右边，不等空值还有or，索引失效

#### 索引下推

先介绍谓词下推

如果有如下的sql语句

~~~sql
select t1.name, t2.name from t1 inner join t2 on t1.id=t2.id;
~~~

这条语句的执行顺序有两种

1. 先执行表的join，再筛选需要的数据
2. 先将所需要的列拿出，再执行表的join

后执行join操作（join为动词）可被理解为谓词下推，减少了表连接前的数据量。

而索引下推

如果筛选条件有name,age

不使用索引下推

- 先根据name列的值将所有数据拉到server层，在server层对age做过滤

使用索引下推

- 根据name，age两个字段把满足要求的数据拉到server层，取出对应的数据

#### MRR

Multi-Range Read，多范围查询

当使用非主键索引查询数据时，没有索引覆盖需要进行回表，即先查询主键值，再到主键索引中查询整行数据。

场景：主键为id，非主键索引为name，根据name去查询，得到的id是无序的（相对于物理顺序），这样根据无序的列表去主键B+树上进行的是随机查找。而如果在name索引树上得到无序的id后，先将id进行排序，当排序完成后在主键索引中查找就不是随机了。

#### 优化原则

小表驱动大表，小的数据库驱动大的数据集。

EXISTS

- SELECT … FROM table WHERE EXISTS(subquery)

语法可理解为：**将主查询的数据，放到子查询中做条件验证，根据验证结果（true或false）来决定主查询数据是否得以保留**，当主查询的表比子查询中表更小时效率高，若主查询中数据多，用IN效率更高。即小表驱动大表。相当于IN查询的变种。

{% asset_img exists.png This is an example image %}

#### order by关键字优化

- 尽量使用index方式排序，避免使用filesort
  - Mysql支持支持两种方式的排序，Filesort和Index，Index效率高，指Mysql扫描索引本身完成排序，Filesort效率较低
  - 两种情况使用Index排序
    - order by语句使用索引最左前列
    - 使用where子句与order by子句条件组合满足索引最左前列

- 尽可能在索引数据列上完成排序，按照索引建的最佳左前缀

  - 若不在索引上，filesort存在双路排序和单路排序

    - 双路排序：Mysql4.1之前采用，扫描**两次**磁盘

    - 单路排序：读取后，在buffer排序，避免第二次读取数据，使用更多空间，将每一行保存进内存

      单路总体比双路好，但存在问题，即如果sort_buffer数据比要全部取的数据小，需要多次IO

- 调优策略

  - 不使用select *，排序顺序最好与索引一致

  - 增大sort_buffer_size
  - 提高max_length_for_sort_data（提高排序查询效率）

{% asset_img orderby.png This is an example image %}

#### group by关键字优化

- group by实质为先排序后分组，按照索引的最佳左前缀原则
- 当无法使用索引列，增大sort_buffer_size+max_length_for_sort_data
- where高于having，能写在where限定条件的不要写在having

## 查询截取分析

### 分析

1. 观察，至少跑一天，看看生产的慢sql情况
2. 开启慢查询日志，设置阈值，如超过5秒钟就为慢sql，将其抓取
3. explain+慢sql分析
4. show profile
5. 运维经理orDBA，进行sql数据库服务器的参数调优

总结

1. 慢查询的开启并捕获
2. explain+慢sql分析
3. show profile查询sql在Mysql服务器中的细节执行细节和生命周期情况
4. SQL数据库服务器参数调优

### 慢查询日志

- 是一种日志记录，用来记录Mysql中响应时间超过阈值的语句，具体为运行时间超过long_query_time值（默认为10秒）的sql，记录到慢查询日志中。
- 默认没有开启，需要手动开启；若不是调优需要，不建议启动，造成性能影响

查看开启

show variables like ‘%slow_query_log%’

设置slow_query_log的值来开启，只对当前数据库生效，Mysql重启后失效；若要用久，需要修改配置文件my.cnf

- set global slow_query_log=1;

查看设定时间

show variables like ’long_query_time%’，判断为**大于**的语句，而不是大于等于

设置阈值时间

set global long_query_time=3;设置3秒

需要重新连接或新开会话才能看到修改值

检验

可以让其休眠等待4s，这样看到效果

select sleep(4);

在慢日志中可以具体看出那条sql语句超出了阈值时间。

查询当前系统中有多少条慢查询记录

show global status like ‘%Slow_queries’;

### Show Profile

mysql提供的用来分析当前会话中语句执行的**资源消耗**情况，可以用于SQL的调优的测量。 	

默认情况下，参数处于关闭状态，并保存15次的运行结果。

1. 看当前是否支持

   show variables like ‘profiling’;默认为关闭，使用前需要开启

2. 设置开启

   set profiling=on;

3. 运行sql

4. 查看结果

   show profiles

   包括Query_ID，Duration，Query

5. 诊断sql

   show profile cpu,block io for query 上一步问题sql号码

   给出每个步骤的具体时间

6. 日常开发需要注意的结论

   - converting HEAP to MyISAM 查询结果太大，内存不够用往磁盘上搬
   - Creating tmp table 创建临时表
     - 拷贝数据至临时表
     - 用完再删除
   - **Copying to tmp table on disk** 把内存中临时表复制到磁盘，危险！！！
   - locked 

### 全局查询日志

永远不要在生产环境开启此功能

编码启动

- set global general_log=1;

- set global log_output=‘TABLE’;

  编写的所有sql语句，会被记录到mysql库里的general_log表，用以下命令查看

- select * from mysql.general_log;

## 锁机制

### 概述

锁是协调计算机多个进程或线程并发访问某一资源的机制

#### 场景

场景：商品一件库存，两个人同时买，谁买到

一定要用到**事务**，从库存表中取出物品数量，插入订单，付款后插入付款表信息，更新商品数量，用锁可以对有限资源进行保护，解决隔离和并发的矛盾。

#### 锁的分类

- 对数据操作类型分类
  - 读锁（共享锁）：针对同一份数据，多个读操作可以同时进行而不会相互影响
  - 写锁（排它锁）：当前写操作没有完成前，会阻断其他写锁和读锁
- 对数据操作粒度分类
  - 表锁（MyISAM）
  - 行锁（InnoDB）
  - 页锁

### 三种锁

表级锁，行级锁与页锁

- 表级锁：开销小，加锁快；不出现死锁；锁定粒度大，发生锁冲突概率最高，并发度最低
- 行级锁：开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突概率最低，并发度最高
- 页面锁（cap锁，间隙锁）：开销和加锁时间介于表锁与行锁之间；会出现死锁；锁定粒度介于表锁与行锁之间，并发度一般

仅从锁的角度，表级锁更适合查询为主的业务，只有少量按照索引条件更新数据的应用

行级锁适合大量按照索引条件并发更新少量不同数据，同时有并发查询的应用

#### 表锁（偏读）

##### 特点

- 偏向MyISAM存储引擎，开销小，加锁快；
- 无死锁；
- 锁定粒度大，发生锁冲突的概率最高，并发度最低

##### 建表

create table 表名(

​	列

)engine myisam;

需要指定执行引擎为myisam

##### 手动增加表锁

lock table 表名字 read(write)，表名字2 read(write)，其他；

show open tables;看哪些有锁

unlock tables;释放表锁

##### 读锁

加锁方

- 可以读自己锁的表
- 不能插入或更新自己锁的表
- 不能读其他表

其他方

- 可以读被锁的表
- 修改被读锁的表，**一直等待**直到获得锁
- 可以读和写其他为被锁的表

##### 写锁

加锁方

- 可以读自己锁的表
- 可以改自己锁的表
- 不能读其他表

其他方

- 可以看和修改其他表
- 查看、修改被写锁的表，**一直等待**直到获得锁

##### 总结

MyISAM执行查询语句前，自动给涉及的所有表加读锁，在执行增删改操作前，自动给涉及的表加写锁

MySQL表级锁有两种模式

- 表共享读锁（Table Read Lock）
- 表独占写锁（Table Write Lock）

| 锁类型 | 可否兼容 | 读锁 | 写锁 |
| ------ | -------- | ---- | ---- |
| 读锁   | 是       | 是   | 否   |
| 写锁   | 是       | 否   | 否   |

1. 对MyISAM表的读操作（加读锁），不会阻塞其他进程对同一表的读请求，但会阻塞对同一表的写请求。只有当读锁释放后，才会执行其他进程的写操作
2. 对MyISAM表的写操作（加写锁），会阻塞其他进程对同一表的读和写操作，只有当写锁释放后，才会执行其他进程的读写操作

即，**读锁会阻塞写，但不会阻塞其他进程读；写锁会把其他进程读和写都阻塞**。

##### 表锁分析

show open tables;看哪些表被锁，为1的是被锁了。

show status like ‘tables%’;

- table_locks_waited：出现表级锁争用而发生等待的次数（不能立即获取锁的次数，等待一次加一），较高说明存在**较严重的表级锁争用**情况
- table_locks_immediate：产生表级锁定的次数，表示可以立即获取锁的查询次数，每立即获取锁值加一

**MyISAM**读写锁的调度是**写优先**，**不适合做写为主表的引擎**。**写锁后，其他线程不能做任何操作，大量更新会使查询很难得到锁，从而造成永远阻塞**。

#### 行锁（偏写）

- 偏向InnoDB引擎，开销大，加锁慢；
- 会出现死锁
- 锁定粒度最小，发生锁冲突的概率最低，并发度也最高

InnoDB与MyISAM最大不同有两点

1. 支持事务（TRANSACTION）
2. 采用了行级锁

##### 建表

create table 表名(

)engine=innodb;

##### 行锁演示

如果两个事务对同一行数据进行更新，则后更新的数据会被阻塞直到第一个事务提交，这时候更新的值为第二个事务的值。如果两个事务对不同行数据进行更新，则不会受到影响。

##### 无索引行锁升级为表锁

若varchar类型的索引，更新数据时，没加单引号将索引做了类型转换，会造成索引失效，从行锁变成表锁。

varchar类型数据一定要加单引号！！！

##### 间隙锁危害

innodb使用间隙锁条件为在可重复读下，检索条件要有索引。

表中的id为1,3,4，如果行锁的条件为where id>1 and id<5，则应该锁的数据是id为3,4的数据，如果此时事务2要插入id=2的数据，按理不应该被锁住，但是会被锁

当用**范围条件**而不是相等条件检索数据，并请求共享或排他锁时，InnoDB会给符合条件的已有数据记录的索引项加锁；对于**键值在条件范围内但并不存在的记录**，叫做“间隙（GAP）”，InnoDB会对这个间隙加锁，锁机制就是间隙锁（Next-Key锁）。

间隙锁的范围是当前查询的条件向左找最靠近检索条件的值，向右找最靠近检索条件的值。不在此范围的数据不会被锁定。

危害

因为Query执行过程中通过范围查找，会锁定整个范围内所有的索引键值，即使这个键值并不存在。

间隙锁有比较致命的弱点，即当锁定一个范围键值后，即使某些不存在的键值也会被无辜的锁定，而造成**在锁定的时候无法插入锁定键值范围内的任何数据**。在某些场景可能对性能造成很大的危害。

##### 如何锁定一行

select xxx for update锁定某一行后，其他的操作会被阻塞，直到锁定行的会话提交commit

~~~sql
begin;
select * from 表名 where 条件 for update;
commit;
~~~

begin开始，语句后加`for update`，最后加commit。

InnoDB存储引擎由于实现了行级锁定，虽然在锁定机制的实现方面所带来的的性能损耗可能比表级锁定要高，但是在整体并发处理能力方面要远远优于MyISAM表级锁定。当系统并发量较高的时候，InnoDB的整体性能和MyISAM相比会有比较明显的优势；但InnoDB行级锁定使用不当时，可能会让InnoDB整体性能还比MyISAM更差。

##### 行锁分析

show status like ‘innodb_row_lock%’;

- InnoDB_row_lock_current_waits;当前正在等待锁定的数量

- InnoDB_row_lock_waits：系统启动后到现在总共等待的次数

若等待次数很高，等待时长不少，用**show profile**分析每一步消耗资源及时间

##### 优化建议

- 尽可能让所有数据检索都通过索引来完成，避免无索引行锁升级为表锁
- 合理设计索引，尽量减少锁的范围
- 尽可能减少检索条件，减少间隙锁
- 尽量控制事务大小，减少锁定资源量和时间长度
- 尽可能低级别事务隔离

#### 页锁

- 开销和加锁时间界于表锁和行锁之间

- 会出现死锁
- 锁定粒度介于表锁和行锁之间，并发度一般

## 主从复制

### 概述

复制为将主库的DDL和DML操作通过二进制日志传递到复制服务器（从库）上，然后从库对这些日志重新执行，从而使得主库和从库数据一致。

#### 主从复制优点

- 若主库出现问题，可以快速切换到从库提供服务
- 可以在从库执行操作，降低主库的访问压力
- 可以在从库进行备份，以免备份期间影响主库的服务

#### 复制解决问题

- 数据分布

- 负载平衡

- 数据备份

- 高可用性和容错性

- 实现读写分离，环节数据库压力

  一般只对更新不频繁和对实时性要求不高的数据可以通过从库查询，实时性要求高的仍要从主库查询

### MySQL主从复制

#### 概念

数据可以从一个MySQL数据库服务器主节点复制到一个或多个从节点。默认采用异步复制方式，数据的更新可以在远程连接上执行，不用一直访问主服务器来更新数据。

#### 主要用途

##### 读写分离

主库负责写，从库负责读，即使主库出现了锁表的情况，通过从库可以保证业务的正常进行

##### 数据备份

数据实时备份，当系统中某个节点发生故障时，可以方便故障切换（主从切换）

##### 高可用

在主服务器上生成实时数据，在从服务器上分析数据，提高主服务器的性能

#### 复制基本原理

{% asset_img 主从复制.png This is an example image %}

- slave会从master读取binblog来进行数据同步
- 三个步骤
  1. master将改变记录到二进制日志（binary log）。记录过程叫做二进制日志文件，binary log events	
  2. slave将master的binary log events拷贝到其中继日志（relay log）
  3. slave重做中继日志中的事件，将改变应用到自己的数据库中，MySQL复制是异步且串行化的

#### 复制基本原则

- 每个slave只有一个master
- 每个slave只能有一个唯一的服务器ID
- 每个master可以有多个slave

#### 一主一从常见配置

保证主从机可以ping通，windows看ipconfig，linux看ifconfig。

- mysql版本一致且后台以服务运行

- 主从都配置在【mysqld】节点下，都是小写

- 主机修改my.ini配置文件（windows）

  1. 【必须】主服务器唯一id   server-id=1

  2. 【必须】启用二进制日志

     log-bin=自己本地路径/mysqlbin

  3. 【可选】启用错误日志

     log-err=自己本地路径/mysqlerr

  4. 【可选】根目录

     basedir=“自己本地路径”

  5. 【可选】临时目录

     tmpdir=“自己的本地路径”

  6. 【可选】数据目录

     datadir=“自己的本地路径/Data/”

  7. read-only=0

     主机，读写都可以

  8. 【可选】设置不要复制的数据库

     binlog-ignore-db=mysql

  9. 【可选】设置需要复制的数据库

     bonlog-do-db=需要复制的主数据库名字

- 从机修改my.cnf配置文件

  - 【必须】从服务器唯一ID
  - 【可选】启动二进制日志

- 主机+从机重启后台mysql服务

- 主机从机关闭防火墙

- 在windows主机上建立账户并授权slave

- 在linux从机上配置需要复制的主机

- 主机新建库，新建表，insert记录，从机复制

- 停止服务复制

  stop slave