---
title:  python 代码片段
key: key-20180201
tags: python
comment: true
---

## 日期相关

### 根据某一天日期获取其前一周的日期

```python
def get_weeks(now_date):
    date_list = []
    dayto = datetime.datetime.strptime(now_date, "%Y-%m-%d")
    for i in range(0,7):
        date_list.append((dayto - datetime.timedelta(days=i)).strftime("%Y-%m-%d"))
    date_list.reverse()
    return date_list
print(get_weeks("2018-08-03"))
#输出：['2018-07-28', '2018-07-29', '2018-07-30', '2018-07-31', '2018-08-01', '2018-08-02', '2018-08-03']
```

### 获取开始和结束日期中间的日期

```python
def getEveryDay(begin_date,end_date):
    date_list = []
    begin_date = datetime.datetime.strptime(begin_date, "%Y-%m-%d")
    end_date = datetime.datetime.strptime(end_date,"%Y-%m-%d")
    while begin_date <= end_date:
        date_str = begin_date.strftime("%Y-%m-%d")
        date_list.append(date_str)
        begin_date += datetime.timedelta(days=1)
    return date_list
print(getEveryDay('2017-05-01','2017-05-06'))
# 输出：['2017-05-01', '2017-05-02', '2017-05-03', '2017-05-04', '2017-05-05', '2017-05-06']
```

### 获取某一天上个星期的日期

```python
import datetime
date = datetime.datetime.now()
def week_get(date):
    dayscount=datetime.timedelta(days=date.isoweekday())
    dayto=d-dayscount
    sixdays=datetime.timedelta(days=6)
    dayfrom=dayto-sixdays
    date_from=datetime.datetime(dayfrom.year, dayfrom.month, dayfrom.day,0,0,0)
    date_to=datetime.datetime(dayto.year, dayto.month, dayto.day,23,59,59)
    print('---'.join([str(date_from),str(date_to)]))
    return date_from,date_to

print(date.isoweekday()) # 这天是星期几

week_get(date)
# 输出：2018-08-06 00:00:00---2018-08-12 23:59:59
```

## list 列表相关

### list dict 排序

```python
items = [
        {'name':'李四','age':'40'},
        {'name':'张三','age':'30'},
        {'name':'王五','age':'50'},
    ]

items1 = sorted(items,key=lambda i:i['age']) 
print(items1) 
# 输出：[{'name': '张三', 'age': '30'}, {'name': '李四', 'age': '40'}, {'name': '王五', 'age': '50'}]
```

## 定时任务

```python
# -*- coding: utf-8 -*-
import datetime,time
from apscheduler.schedulers.background import BackgroundScheduler
from apscheduler.schedulers.blocking import BlockingScheduler

def job():
    print('Tick! The time is: %s' % datetime.datetime.now())

if __name__ == '__main__':
    
    scheduler = BackgroundScheduler() # 后台执行的定时任务
    sched = BlockingScheduler() # 主进程阻塞的定时任务，即定时任务开始后，主进程不往下继续进行
    
    # 间隔3秒钟执行一次
    scheduler.add_job(job, 'cron', hour='*', minute='*', second='*/3')   
   
    sched.add_job(job, 'cron', hour='*', minute='*', second='10')

    # 这里的调度任务是独立的一个线程
    scheduler.start()
#    sched.start() # 这个会阻塞主进程
    print("定时工作开始",datetime.datetime.now())
    print('Press Ctrl+C to exit')
    try:
        # 其他任务是独立的线程执行
        while True:
            time.sleep(2)
            print('sleep!')
    except (KeyboardInterrupt, SystemExit):
        scheduler.shutdown()
        sched.shutdown()
        print('Exit The Job!')
```