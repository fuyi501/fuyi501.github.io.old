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

上述代码完成的功能即是打印通过命令行运行python脚本代码时传递的参数，python脚本中使用了sys模块。这个模块中的argv属性是一个list，存放使用系统命令行运行python脚本时传入的参数和脚本文件的名称，当然argv的第一个值即是脚本名称，从第二个值往后才是命令行传入的参数，上述代码运行效果如下: 

在nodejs中需要实现调用这个脚本，那么相应的 javaScript 代码如下:

```js
var exec = require('child_process').exec;
var arg1 = 'hello'
var arg2 = 'world'
var filename = 'test_py.py'
exec('python'+' '+filename+' '+arg1+' '+arg2,function(err,stdout,stderr){
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

我们可以看到从python脚本输出到控制台的内容在nodejs的程序中被完全解析为字符串，存放于回调函数的输入参数stdout中。因此如果我们需要实现nodejs脚本调用python脚本并且获取python脚本输出的结果时可以选择在python脚本中对计算结果进行打印，然后在nodejs的脚本中对这个打印的字符串进行解析即可。 
javascript中处理的对象多是JSON。因此要实现友好的两种脚本语言交互可以在python中先对要交互的内容生成json字符串,然后使用print打印输出，而javascript 代码获取这个字符串后可以直接进行json对象转换。实例如下: 
首先是python代码,实现将json对象转换为字符串输出到控制台:

```python
import json # import the module of json
import sys # this module is used to get the params from cmd
params = sys.argv[1]
obj = json.loads(params) #str to obj
jsonob = {'name':'alex','age':18,'gender':'male'}
strjson = json.dumps(jsonob,sort_keys=True)
print(strjson) 
```

python代码接收来自于命令行的参数，然后输出一个json对象对应的字符串。 
javascript代码实现将python脚本输出的json字符串转换为json对象如下:

```js
var exec = require('child_process').exec;
filename = 'test_pyjson.py'
var editor={
    "name":'alex',
    "age":18,
    "address":'fghjh'
};
var str = '{\\"name\\":\\"alex\\",\\"age\\":18,\\"address\\":\\"sdsdd\\"}'; //change the javascriptobject to jsonstring
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

可以看到javascript成功的解析Python代码执行的命令行输出结果，实现了javascript与python的混编。



## 小结

nodejs 调用脚本与其他脚本的交互过程主要就是三步：javascript代码中使用child_process模块创建子进程，子进程调用命令行并且传递参数完成其他语言脚本代码的调用，根据其他语言的控制台输出的字符串进行JSON格式的解析，进而完成了Nodejs与其他脚本语言的交互过程。
