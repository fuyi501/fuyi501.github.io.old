---
title: python2.7 使用 mysql 数据库
key: 20170802
tags: 数据库 python
comment: true
---

## python 数据库 api 对象

在 python 中，和 mysql 数据库有关的对象主要有三个，就是

- connection连接对象
- 数据库交互对象 cursor
- 异常对象 exceptions

![enter description here][1]

## 访问数据库基本流程

![enter description here][2]

## 数据库连接对象 connection

使用 connect 方法来连接 mysql 数据库，connect 参数如下：

![enter description here][3]

connection 对象有以下四个主要的方法，具体用法下面再讲。

![enter description here][4]

### 数据库连接实例代码：

```python
# -*- coding: UTF-8 -*-

import MySQLdb as mdb

conn = mdb.connect(
  host='127.0.0.1', 
  port=3306, 
  user='root',
  passwd='123456', 
  db='test', 
  charset='utf8'
  )

cursor = conn.cursor()

print cursor
print conn
cursor.close()
conn.close()

```

## 数据库游标对象 cursor

### cursor 对象主要有以下几个方法

![enter description here][5]

### execute 方法

![enter description here][6]

### fetch() 方法

![enter description here][7]

## python 操作数据库实例演示

### select 查询数据

![enter description here][8]

首先需要准备好一个数据库，数据库名为 `test`，数据库中有 `students` 表，我们先插入了一些数据，如下图所示：

![enter description here][9]

### 实例代码：

```python
# -*- coding: UTF-8 -*-
import MySQLdb as mdb

# 也可以使用关键字参数
conn = mdb.connect(
  host='127.0.0.1', 
  port=3306, 
  user='root',
  passwd='123456', 
  db='test', 
  charset='utf8'
  )

cursor = conn.cursor()
sql = "select * from students"
cursor.execute(sql)
print cursor.rowcount
rs = cursor.fetchall()
for row in rs:
  print "id = %d , name=%s , sex=%s" % row

rs = cursor.fetchone()
print rs
rs = cursor.fetchmany(3)
print rs
rs = cursor.fetchall()
print rs
cursor.close()
conn.close()
```

### 增删改查 数据库

#### 增删改查数据库的一般流程

![enter description here][10]

#### 什么是事务？

![enter description here][11]

举例来说明commit，rollback方法怎么使用

```python
# -*- coding: UTF-8 -*-

import MySQLdb as mdb

conn = mdb.connect(
  host='127.0.0.1', 
  port=3306, 
  user='root',
  passwd='123456', 
  db='test', 
  charset='utf8'
  )

cursor = conn.cursor()

sql = "select * from students"

sql_insert = 'insert into students(id,sname,sex) values(7, "sdf", "男")'
sql_update = 'update students set sname="李雄" where id=4'
sql_delete = 'delete from students where id<3'

cursor.execute(sql)  # 执行查询语句
print cursor.rowcount  # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变
cursor.execute(sql_insert)  # 执行插入语句
print cursor.rowcount   # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变
cursor.execute(sql_update)  # 执行更新语句
print cursor.rowcount   # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变
cursor.execute(sql_delete)  # 执行删除语句
print cursor.rowcount   # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变

cursor.close()
conn.close()
```

在上面的代码中，我们执行了 4 条sql语句，执行结果为 `5、 1、 1、 1`，因为我们的数据库中只有 5 条数据，所以第一条查询语句生效的有 5 条数据，后面三条语句，导致数据库更改的都只有一条数据，所以输出都为 1 。

但是这时，我们来看数据库，发现数据库并没有改变：

![enter description here][12]

这是为什么呢？原因就是我们需要提交事务，使用 commit 方法提交 sql 语句的改变，才能真正更改数据库。如下代码，添加一条语句  `conn.commit()`，再执行一次

```python
# 上面代码省略，如上面一致。

cursor.execute(sql)  # 执行查询语句
print cursor.rowcount  # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变
cursor.execute(sql_insert)  # 执行插入语句
print cursor.rowcount   # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变
cursor.execute(sql_update)  # 执行更新语句
print cursor.rowcount   # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变
cursor.execute(sql_delete)  # 执行删除语句
print cursor.rowcount   # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变

conn.commit()

cursor.close()
conn.close()
```
执行之后，再来看数据库中的数据，发生了变化：

![enter description here][13]

这个时候，我们可能会想，如果执行语句有错误怎么办？如果有错误，这样执行了 commit 提交事务，那岂不是破坏了数据库，所以这个时候就需要数据库异常类，当执行sql 语句发生异常时，就需要执行 rollback 方法回滚当前事务，也就是把刚才执行的 sql 语句全部取消，回退到未执行之前的状态，这样就保护了数据库的安全。所以在执行 sql 语句时，我们都需要使用 `try except ` 把数据库操作语句包含起来，来捕获异常。正常的完整代码如下所示：

```python
# -*- coding: UTF-8 -*-

import MySQLdb as mdb

conn = mdb.connect(
  host='127.0.0.1', 
  port=3306, 
  user='root',
  passwd='123456', 
  db='test', 
  charset='utf8'
  )

cursor = conn.cursor()

sql = "select * from students"

sql_insert = 'insert into students(id,sname,sex) values(7, "sdf", "男")'
sql_update = 'update students set sname="李雄" where id=4'
sql_delete = 'delete from students where id<3'

try:
  cursor.execute(sql)  # 执行查询语句
  print cursor.rowcount  # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变
  cursor.execute(sql_insert)  # 执行插入语句
  print cursor.rowcount   # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变
  cursor.execute(sql_update)  # 执行更新语句
  print cursor.rowcount   # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变
  cursor.execute(sql_delete)  # 执行删除语句
  print cursor.rowcount   # 查看执行 execute 方法导致mysql数据库有多少条数据发生了改变

  conn.commit() # 提交事务
except Exception as e:  # 捕获异常
  print e
  conn.rollback()  # 回滚事务

cursor.close()
conn.close()

```


  [1]: http://images.fuyix.cn/pythonApi.png "pythonApi"
  [2]: http://images.fuyix.cn/python%E8%AE%BF%E9%97%AE%E6%95%B0%E6%8D%AE%E5%BA%93%E6%B5%81%E7%A8%8B.png "python访问数据库流程"
  [3]: http://images.fuyix.cn/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%9E%E6%8E%A5%E5%AF%B9%E8%B1%A1connection.png "数据库连接对象connection"
  [4]: http://images.fuyix.cn/connection%E5%AF%B9%E5%BA%94%E7%9A%84%E6%96%B9%E6%B3%95.png "connection对应的方法"
  [5]: http://images.fuyix.cn/%E6%B8%B8%E6%A0%87%E5%AF%B9%E8%B1%A1cursor.png "游标对象cursor"
  [6]: http://images.fuyix.cn/execute%E6%96%B9%E6%B3%95.png "execute方法"
  [7]: http://images.fuyix.cn/fetch%E6%96%B9%E6%B3%95%E4%BB%8B%E7%BB%8D.png "fetch方法介绍"
  [8]: http://images.fuyix.cn/select%E6%9F%A5%E8%AF%A2%E6%95%B0%E6%8D%AE.png "select查询数据"
  [9]: http://images.fuyix.cn/python-mysql%E6%95%B0%E6%8D%AE%E5%BA%93%E5%AE%9E%E4%BE%8B.png "python-mysql数据库实例"
  [10]: http://images.fuyix.cn/python-%E5%A2%9E%E5%88%A0%E6%94%B9%E6%9F%A5%E6%95%B0%E6%8D%AE%E5%BA%93.png "python-增删改查数据库"
  [11]: http://images.fuyix.cn/python-%E6%95%B0%E6%8D%AE%E5%BA%93-%E4%BA%8B%E5%8A%A1.png "python-数据库-事务"
  [12]: http://images.fuyix.cn/python-%E6%95%B0%E6%8D%AE%E5%BA%93%E6%9C%AA%E6%9B%B4%E6%94%B9.png "python-数据库未更改"
  [13]: http://images.fuyix.cn/python-%E6%95%B0%E6%8D%AE%E5%BA%93%E6%9B%B4%E6%94%B9.png "python-数据库更改"