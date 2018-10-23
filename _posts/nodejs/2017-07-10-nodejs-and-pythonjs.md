---
title: nodejs 中调用 python 脚本
key: 20170710
tags: nodejs python
comment: true 
---

## 使用 python.js 

python.js 是一个在 node.js 中调用 Python 代码的模块，项目地址：[https://github.com/monkeycz/python.js](https://github.com/monkeycz/python.js)

## 使用 child_process

文档地址：[http://nodejs.cn/api/child_process.html](http://nodejs.cn/api/child_process.html)

首先写一个简单的 python 测试脚本 test.py 代码如下:

```python
#encode:utf-8
import sys
for i in range(len(sys.argv)):
    print('arg'+str(i),sys.argv[i])
```

上述代码完成的功能即是打印通过命令行运行 python 脚本代码时传递的参数，python脚本中使用了sys 模块。这个模块中的 argv 属性是一个 list，存放使用系统命令行运行 python 脚本时传入的参数和脚本文件的名称，当然 argv 的第一个值即是脚本名称，从第二个值往后才是命令行传入的参数，上述代码运行效果如下: 

在 nodejs 中需要实现调用这个脚本，那么相应的 javaScript 代码如下:

```js
var exec = require('child_process').exec;
var arg1 = 'hello'
var arg2 = 'world'
var filename = 'test_py.py'
exec('python'+' '+filename+' '+arg1+' '+arg2, function (err,stdout,stderr) {
    if(err)
    {
        console.log('stderr',err);
    }
    if(stdout)
    {
        console.log('stdout',stdout);
    }
});
```

最后在命令行下执行的结果为: 

我们可以看到从 python 脚本输出到控制台的内容在 nodejs 的程序中被完全解析为字符串，存放于回调函数的输入参数 stdout 中。因此如果我们需要实现 nodejs 脚本调用 python 脚本并且获取 python 脚本输出的结果时可以选择在 python 脚本中对计算结果进行打印，然后在 nodejs 的脚本中对这个打印的字符串进行解析即可。 

javascript 中处理的对象多是 JSON。因此要实现友好的两种脚本语言交互可以在 python 中先对要交互的内容生成 json 字符串，然后使用 print 打印输出，而 javascript 代码获取这个字符串后可以直接进行 json 对象转换。

实例如下: 
首先是 python 代码，实现将 json 对象转换为字符串输出到控制台：

```python
import json # import the module of json
import sys # this module is used to get the params from cmd
params = sys.argv[1]
obj = json.loads(params) #str to obj
jsonob = {'name':'alex','age':18,'gender':'male'}
strjson = json.dumps(jsonob,sort_keys=True)
print(strjson) 
```

python 代码接收来自于命令行的参数，然后输出一个 json 对象对应的字符串。 
javascript 代码实现将 python 脚本输出的 json 字符串转换为 json 对象如下：

```js
var exec = require('child_process').exec;
filename = 'test_pyjson.py'
var editor={
    "name":'alex',
    "age":18,
    "address":'fghjh'
};
var str = '{\\"name\\":\\"alex\\",\\"age\\":18,\\"address\\":\\"sdsdd\\"}'; // change the javascriptobject to jsonstring
exec('python '+filename+' '+str,function(err,stdout,stdin){
    if(err){
        console.log('err');
    }
    if(stdout)
    {
        //parse the string
        console.log(stdout);
        var astr = stdout.split('\r\n').join('');//delete the \r\n
        var obj = JSON.parse(astr);

        console.log('name',obj.name);
        console.log('age',obj.age);
        console.log('gender',obj.gender);
    }
});
```

可以看到 javascript 成功的解析 Python 代码执行的命令行输出结果，实现了 javascript 与 python 的混编。

## 小结

nodejs 调用脚本与其他脚本的交互过程主要就是三步：javascript 代码中使用 child_process 模块创建子进程，子进程调用命令行并且传递参数完成其他语言脚本代码的调用，根据其他语言的控制台输出的字符串进行 JSON 格式的解析，进而完成了 Nodejs 与其他脚本语言的交互过程。
