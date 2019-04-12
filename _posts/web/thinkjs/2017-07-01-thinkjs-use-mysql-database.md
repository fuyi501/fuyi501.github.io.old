---
title: thinkjs 操作 mysql 数据库
key: 20170701
tags: thinkjs
comment: true
---

> thinkjs 操作数据库实际上也是很简单的一件事，按照官方网站的介绍，我们需要新建一个模型，在模型中封装对数据库的操作，这个是一种好的方法，可以很清晰的分离出对数据库的操作层，这次我们先来介绍一个简单的方法，只需要在控制器和视图中操作就可以了。

## 准备数据库

首先在 mysql 中创建一个数据库 `test`，然后新建一个表 `students`，插入一些数据，如下所示：

```sql
create database test;
use test;
create table students(
	id int(10) ,
    name varchar(10) ,
    sex varchar(4) ,
    phone varchar(15)
)default charset utf8;

insert into test.students value(1,'张三','男','12345678');
insert into test.students value(2,'张三','男','12345678');
insert into test.students value(3,'张三','男','12345678');
insert into test.students value(4,'张三','男','12345678');
insert into test.students value(5,'张三','男','12345678');
insert into test.students value(6,'张三','男','12345678');
insert into test.students value(7,'张三','男','12345678');
insert into test.students value(8,'张三','男','12345678');
```

有了 mysql 数据库后就该配置 thinkjs 和使用 thinkjs 操作 mysql 了。

## thinkjs 配置 mysql

在 thinkjs 中配置 mysql ，非常的简单，打开 `src/common/config/db.js`，默认配置如下：

```js
export default {
  type: 'mysql',
  adapter: {
    mysql: {
      host: '127.0.0.1',
      port: '',  // 端口号
      database: '',  // 数据库名字
      user: '',  // 数据库账号
      password: '',  // 数据库密码
      prefix: '',  // 数据库前缀
      encoding: 'utf8'
    },
    mongo: {

    }
  }
};
```

只需要根据自己的数据库填写上面的内容即可，填好之后再打开 thinkjs 项目，就可以访问我们的数据库了，更详细的内容请参考 [thinkjs官网 数据库配置][1]

## 操作 mysql

我们还是只需要 操作控制器和视图就可以了。

修改 ` src/home/controller/index.js ` 中的 ` indexAction ` 方法，这个 ` indexAction ` 就是一个操作，这个方法就是用来处理 ` index ` 页面的，在这个方法中使用 `this.model('数据库下表的名字')` 就可以连接数据库了。

注意：` indexAction ` 方法默认是不带 `async` 参数的，这是为了配合 方法中的 `await` 使用的，将 nodejs 的异步执行改为 同步执行。

下面我们来尝试一下：

```js
async indexAction(){
  // auto render template file index_index.html
  this.assign("title","Hello Thinkjs") // 给title赋值为 Hello Thinkjs

  let stu = await this.model('students').select();
  
  console.log("stu:",stu);

  return this.display();
}
```

刷新页面，输出：

```bash
stu: _class {
  pk: 'id',
  name: 'students',
  tablePrefix: undefined,

  tableName: '',
  schema: {},
  indexes: {},
  config:
   { type: 'mysql',
     host: '127.0.0.1',
     port: '3306',
     database: 'test',
     user: 'root',
     password: '123456',
     prefix: '',
     encoding: 'utf8',
     nums_per_page: 10,
     log_sql: true,
     log_connect: true,
     camel_case: false,
     cache: { on: true, type: '', timeout: 3600 },
     schema_force_update: true },
  _db: null,
  _data: {},
  _options: {} }
```

说明返回的 stu 是一个数据表对象，官网称为 `模型实例化对象` 。下面来操作一下这个数据表。我们可以使用 `select` 方法查询多条数据，返回值为数据。更多操作方法，请看 [thinkjs官网 CRUD 操作][2]。

代码示例：

```js
async indexAction(){
  //auto render template file index_index.html
  this.assign("title","Hello Thinkjs") //给title赋值为 Hello Thinkjs

  let stu = await this.model('students').select();

  console.log("stu:",stu);

  return this.display();
}
```

刷新页面输出：

```bash
stu: [ RowDataPacket { id: 1, name: '张三', sex: '男', phone: '12345678' },
  RowDataPacket { id: 2, name: '张三', sex: '男', phone: '12345678' },
  RowDataPacket { id: 3, name: '张三', sex: '男', phone: '12345678' },
  RowDataPacket { id: 4, name: '张三', sex: '男', phone: '12345678' },
  RowDataPacket { id: 5, name: '张三', sex: '男', phone: '12345678' },

  RowDataPacket { id: 6, name: '张三', sex: '男', phone: '12345678' },
  RowDataPacket { id: 7, name: '张三', sex: '男', phone: '12345678' },
  RowDataPacket { id: 8, name: '张三', sex: '男', phone: '12345678' } ]
```

可以看出来 ，返回值是一个 json 数据。有了 json 数据，我们需要将他传递到前端页面，在前端页面上展示这些数据，这个和上一篇文章中的 title 的赋值是一样的。我们使用 `this.assign()` 方法，将 json 数据传递到前端页面，如下：

```js
async indexAction(){
  //auto render template file index_index.html
  this.assign("title","Hello Thinkjs") //给title赋值为 Hello Thinkjs

  let stu = await this.model('students').select();

  console.log("stu:",stu);

  this.assign("stu_infos",stu)

  return this.display();
}
```

## 前端展示 mysql 数据

上面说 通过使用 ` this.assign("stu_info",stu)` 方法，讲返回的 stu 数据赋值给 stu_info 并传递到了前端页面，那到底有没有呢？
下面我们配合 ` nunjucks` 模板引擎，在前端把刚刚查询到的 mysql 数据库中的数据展示出来。 ` nunjucks` 模板引擎 的使用方法请参考 [thinkjs官网 nunjucks模板引擎][3]。

打开 ` view/home/index_index.html ` ，我们删掉一些不用的，使其看起来简单一些，如下：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>New ThinkJS Application</title>
<style>
*{padding: 0;margin: 0;font-size: 16px;line-height: 20px;font-family: arial;}
a, a:visited{color:#337ab7;text-decoration: none;}
header{padding: 70px 0 70px 0;background-color: #4A6495}
h1{font-size: 36px;color:#fff;font-weight: normal;}
h3{font-size: 26px;color:#fff;font-weight: normal;margin-top: 40px;}
code{  padding: 2px 4px;font-size: 90%;color: #c7254e;background-color: #f9f2f4;border-radius: 4px;}
.content{width: 1000px;margin: auto}
.wrap{width: 1000px;margin: auto}
.content{margin-top: 80px;}
.list{width: 800px;}
.list .item{position: relative;padding-left: 70px;margin-top: 50px;}
.list .item .step{position: absolute;width: 36px;height: 36px;top:-3px;left:0;border: 5px solid #4A6495;border-radius: 23px;text-align: center;line-height: 36px;}
.list .item h2{font-size: 24px;font-weight: normal;}
.list .item p{line-height: 30px;margin-top: 10px}
</style>
</head>
<body>
  <header>
    <div class="wrap">
      <h1>A New App Created By ThinkJS</h1>
      <h3>{{ title }}</h3>
    </div>
  </header>
  <div class="content">
    <div class="list">
      <div class="item">
        <div class="step">1</div>
      </div>
    </div>
  </div>
</body>
</html>
```

下面我们来使用 ` nunjucks` 模板引擎 ，关键代码如下，其他代码不变。

```html
<div class="content">
  <div class="list">
    {% for stu in stu_infos %}
    <div class="item">
      <div class="step"> {{ stu.id }} </div>
      <p>
        {{ stu.name }} {{ stu.sex }} {{ stu.phone }}
      </p>
    </div>
    {% endfor %}
  </div>
</div>
```
  
 刷新页面，展示如下：
 
 ![enter description here][4]
 
 查询出来的八条数据全部展示出来了，到这里 thinkjs 操作 mysql 数据库的所有步骤就结束了。预知更多信息，请看 [官方文档][5]


  [1]: https://thinkjs.org/zh-cn/doc/2.2/model_config.html#toc-a35
  [2]: https://thinkjs.org/zh-cn/doc/2.2/model_crud.html
  [3]: https://thinkjs.org/zh-cn/doc/2.2/view.html#nunjucks
  [4]: http://images.fuyix.cn/thinkjs%E5%89%8D%E7%AB%AF%E5%B1%95%E7%A4%BA.png "thinkjs前端展示"
  [5]: https://thinkjs.org/zh-cn/doc/index.html