---
title: mysql创建数据库并插入数据
key: 20170801
tags: 数据库 mysql
comment: true
---

后续的笔记都在 MySQL Workbench 的中进行操作。

# 新建数据库

语句格式为 `CREATE DATABASE <数据库名字>;` ，（注意不要漏掉分号 ;），前面的 `CREATE DATABASE` 也可以使用小写，创建名为 test 的数据库，具体命令为：

	CREATE DATABASE test; 

`show databases; ` 用来查看数据库。

在大多数系统中，SQL 语句都是不区分大小写的，因此以下语句都是合法的：
```mysql
CREATE DATABASE name1;
create database name2;
CREATE database name3;
create DAtabaSE name4;
```
但是出于严谨，而且便于区分保留字（保留字(reserved word)：指在高级语言中已经定义过的字，使用者不能再将这些字作为变量名或过程名使用。）和变量名，我们把保留字大写，把变量和数据小写。

## 连接数据库

要想使用刚创建的数据库，需要先连接数据库，使用语句 `use <数据库名字>` ，连接数据库后，才能在数据库中 创建 表 等后续操作。

	use test;
	
`show tables; ` 用来查看表

## 数据表

数据表（table）简称表，它是数据库最重要的组成部分之一。数据库只是一个框架，表才是实质内容。

而一个数据库中一般会有多张表，这些各自独立的表通过建立关系被联接起来，才成为可以交叉查阅、一目了然的数据库。如下便是一张表：

|  ID   |  name   |  phone   | 
| --- | --- | --- | 
|  01  | Tom    |  110110110   |   
|  02  | Jack   |  119119119   | 
|  03  | Rose  |  114114114   |  

## 新建数据表

在数据库中新建一张表的语句格式为：
```
CREATE TABLE 表的名字
(
列名a 数据类型(数据长度),
列名b 数据类型(数据长度)，
列名c 数据类型(数据长度)
);
```
我们尝试在 test 中新建一张表 student，包含学号 id，姓名 name ，性别 sex 和 电话 等信息，所以语句为：

```mysql
use test;

create table students(
	id int(10) ,
    name varchar(10) ,
    sex varchar(4) ,
    phone varchar(15)
);
```