---
title: python 使用 mysql 数据库银行转账实例
key: 20170803
tags: 数据库 python
comment: true
---

## 创建一个数据库

```sql
create table `account` (
		`acct_id` int(11) default null comment '账户ID',
		`money` int(11) default null comment '余额'
)engine=InnoDB default charset=utf8;  -- 不能设置成 MyISAM

insert into test.account values(11, 110);
insert into test.account values(12, 10);
```

## 完整代码

```python
# coding:utf8  #别忘了设置python编码
import sys
import MySQLdb as mdb

class TransferMoney(object):
    def __init__(self, conn):
        self.conn = conn

    def check_account(self, acctid): # 检查账户是否可用
        cursor = self.conn.cursor()
        try:
            sql = "select * from account where acct_id=%s" % acctid
            print '检查账户是否可用:' + sql
            cursor.execute(sql)
            rs = cursor.fetchall()
            if len(rs) != 1:
                raise Exception("账号 %s 不存在" % acctid)
        finally:
            cursor.close()

    def has_enough_money(self, acctid, money):  # 检查账户是否有足够的钱

        cursor = self.conn.cursor()
        try:
            sql = "select * from account where acct_id=%s and money>%s" % (acctid , money)
            print '检查账户是否有足够的钱:' + sql
            cursor.execute(sql)
            rs = cursor.fetchall()
            if len(rs) != 1:
                raise Exception("账号 %s 没有足够的钱" % acctid)
        finally:
            cursor.close()

    def reduce_money(self, acctid, money): # 对账户的钱进行更改
        cursor = self.conn.cursor()
        try:
            sql = "update account set money=money-%s where acct_id=%s" % (money, acctid)
            print '原账户减去对应的转账金额:' + sql
            cursor.execute(sql)
            rs = cursor.rowcount  # 查看 sql 语句是否更改了一行
            if rs != 1:
                raise Exception("账号 %s 减款失败" % acctid)
        finally:
            cursor.close()

    def add_money(self, acctid, money):  # 对账户的钱进行更改
        cursor = self.conn.cursor()
        try:
            sql = "update account set money=money+%s where acct_id=%s" % (money, acctid)
            print '目标账户加上对应的转账金额:' + sql
            cursor.execute(sql)
            rs = cursor.rowcount  # 查看 sql 语句是否更改了一行
            if rs != 1:
                raise Exception("账号 %s 加款失败" % acctid)
        finally:
            cursor.close()

    def transfer(self, source_id, target_id, money):

        try:
            self.check_account(source_id)  # 检查原账户是否存在
            self.check_account(target_id)  # 检查目标账户是否存在
            self.has_enough_money(source_id, money)  # 检查原账户是否有足够的钱
            self.reduce_money(source_id, money)  # 对原账户进行减操作
            self.add_money(target_id, money)  # 对目标账户进行加操作

            self.conn.commit()  # 提交事务

        except Exception as e:
            self.conn.rollback()
            raise e

if __name__ == '__main__':
    source_id = sys.argv[1]
    target_id = sys.argv[2]
    money = sys.argv[3]

    conn = mdb.connect(
        host='localhost',
        port=3306,
        user='root',
        passwd='123456',
        db='test',
        charset='utf8')

    transfer_money = TransferMoney(conn)

    try:
        transfer_money.transfer(source_id, target_id, money)
        print '转账成功'
    except Exception as e:
        print '转账出错：' + str(e)
    finally:
        conn.close()
```
