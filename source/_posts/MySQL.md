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

- 将数据放到**表**中，表再放到库中
- 一个数据库可以有多个表，每个表有唯一性的用于标识的表名
- 表的**特性**定义了数据在表中如何存储，类似java中“类”的设计
- 表由**列**组成，也称为字段。所有表都是由一个或多个列组成，每一列类似java中的“**属性**”
- 表中的数据按**行**存储，每一行类似java中的“**对象**”

## MySQL数据库介绍

​	MySQL是一种开源的关系型数据库管理系统，广泛应用在中小型网站中。

### DBMS分类

- 基于共享文件系统的DBMS（Access）

- 基于客户机--服务器的DBMS   C/S  （MySQL，Oracle，SqlServer）

  主要安装服务端。

### MySQL安装与启动

​	默认端口号3306，要先开启服务端，以管理员身份开cmd输入net start mysql5.5，后面为自定义的mysql服务名，然后开启mysql服务，普通用户打开cmd，输入mysql -u用户名 -p密码，即可。

使用exit退出，也可以mysql -uroot -p，然后再输入密码，这样可以隐藏密码。

连接其他主机上的数据库

~~~
mysql -h主机名 -P端口号 -u用户名 -p密码
~~~

## SQL

### demo：创建表并添加信息

命令要以分号 ; 或者 \g 结尾，建议;

- show databases; 看数据库有哪些

  - information_schema 服务端基本信息数据

  - mysql 用户信息，表信息等

  - performance_schema 性能分析

    **前三个都不要改动！！！**

  - test 

    默认为空

- use mysql; 使用某个数据库

- show tables; 显示表
  
- show tables from test; 直接看某个数据库的表 不影响当前库，因为**没有使用use切换库**。
  
- select database();查看当前所在库

- 建立类来存学生信息

  ~~~sql
  create table stuinfo(  换行
  //相当于新建类
  stuid int,  //学生id，int类型，用逗号隔开
  stuname varchar(20),  //学生姓名，字符串或者字符均为varchar类型，需要 指定长度
  gender char,  //性别，单个字符
  borndate datetime);   //生日，不存年龄是因为年龄会变，最后一个加反括号和分号结束
  ~~~

- desc stuinfo; 看表结构  describe

- select * from stuinfo; 查看表中数据

- set names gbk; 更改服务端编码集

- insert into stuinfo values(1,’张无忌’,’男’,’1998-3-3’);

  插入数据，如果客户端与服务端编码格式不符，需要更改，除了数字均用引号括起来

- update stuinfo set borndate=‘1980-1-1’ where stuid = 2;

  更新表格设置生日，只更改id=2的。

- delete from stuinfo where stuid = 1;

  删除id=1的数据。

- alter table stuinfo add column email varchar(20);

  alter(改变)修改表结构，添加列column email 类型为varchar(20)。

- drop table stuinfo; 干掉表

### 语法注意事项

1. 不区分大小写，但建议关键字大写，表名、列名小写

2. 每条命令最好用分号结尾

3. 每条命令如果根据需要，可以进行缩进或换行

   关键字最好单独一行

4. 注释
   - 单行注释：#注释文字
   - 单行注释：-- 注释文字，中间要有空格
   - 多行注释：/* 注释文字 */

### 分类

- DDL（Data Definition Language）：数据定义语言，用来定义数据库对象：库、表、列等；

  create/drop/alter

- **DML**（Data Manipulation Language）：数据操作语言，用来定义数据库记录（数据）；

  **insert/update/delete**

- DCL（Data Control Language）：数据控制语言，用来定义访问权限和安全级别；

  TCL(Transaction Control Language)

- **DQL**（Data Query Language）：数据查询语言，用来查询记录（数据）。

  **select**