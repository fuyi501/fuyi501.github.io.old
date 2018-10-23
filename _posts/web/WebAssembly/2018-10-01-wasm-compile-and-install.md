---
title: wasm 编译和安装
key: key-20181001
tags: wasm webAssembly emsdk c++
comment: true 
---

`javascript` 这门语言的性能无法与原生的 `C/C++` 代码相媲美，为了进一步提高网页的性能，业界推出了 `WebAssembly` 技术：将C语言编译成了浏览器可以执行的 `wasm` 文件。`WebAssembly` 不仅提高了网页的性能，而且还可以完成原来 `javascript` 无法实现的功能。

`wasm` 是 `WebAssembly` 格式的浏览器可执行文件。它是二进制的，但是它并不像桌面 `win32` 程序一样，可以随便使用系统资源，调用操作系统api。事实上，所有与外界相关的操作都必须由 `javascript` 传入。比如：要申请一段内存，必须由 `javascript` 申请了并传给他。 `浏览器上，javascript` 做不到的，它也做不到；`javascript` 能做到的，它能做的更快。这个就是它的价值。

要想使用 `WebAssembly`，就需要通过 `Emscripten` 编译器将C语言编译成 `wasm` 文件，但是[官方安装教程](https://kripken.github.io/emscripten-site/docs/getting_started/downloads.html)不够清楚，由于国外网速慢，执行命令很容易下载失败，有必要再总结下安装过程顺便说一下解决方法。

官网：[https://kripken.github.io/emscripten-site/index.html](https://kripken.github.io/emscripten-site/index.html)

安装系统：ubuntu 16.04 

## 下载和安装

```sh
# Get the emsdk repo
git clone https://github.com/juj/emsdk.git

# Enter that directory
cd emsdk

# Fetch the latest version of the emsdk (not needed the first time you clone)
git pull

# Download and install the latest SDK tools.
./emsdk install latest

# Make the "latest" SDK "active" for the current user. (writes ~/.emscripten file)
./emsdk activate latest

# Activate PATH and other environment variables in the current terminal
source ./emsdk_env.sh
```

在执行 `emsdk install latest` 会下载安装很多东西，有一个东西300多M，又由于是从国外下载，下载很容易出错，不过你可以提前下载好，放在命令行提示的保存路径 zips 下，再执行这条命令就可以解决问题。

## 安装成功如下图所示

![mark](http://images.fuyix.cn/blog/181009/Jk5IkgIg84.png?imageslim)

![mark](http://images.fuyix.cn/blog/181009/ekC2mcBAL1.png?imageslim)

![mark](http://images.fuyix.cn/blog/181009/8HaFhmmiJa.png?imageslim)

![mark](http://images.fuyix.cn/blog/181009/Fd0B6mib5B.png?imageslim)

## 测试编译简单的 c++/c 代码

```c++
#include <stdio.h>

int main() {
  printf("hello, world!\n");
  return 0;
}
```

```sh
./emcc hello_world.c
node a.out.js
```

默认情况下，emcc 只输出了一个 js（asmjs）。asmjs 是 webassembly 的一个早期原型，可提供webassembly 在旧版本浏览器上的兼容。按如下命令输出 webassembly 二进制 wasm 。

```c++
./emcc hello_world.c -s WASM=1 -o index.html
```

这次编译输出了 `index.html, index.js, index.wasm` 三个文件。通过一个静态服务器打开index.html，可以看到 console 里的输出。

![mark](http://images.fuyix.cn/blog/181009/G63E43hij2.png?imageslim)

这个 index.html 是一个调试页面。生产上加载 webassembly 一般都需要自己写 index.html，只保留js 和 wasm 文件就够了。

以上的例子中，printf 的标准输出被定向到了浏览器的 console 里面。 系统API 调用被换成了 js 实现。 事实上很多 libc 里面的函数被 emscripten 实现成了浏览器上的兼容方案，从而更好的和浏览器结合。

## Emsctipten API
Emscripten sdk 提供了很多API与js运行环境／浏览器交互。参考[https://segmentfault.com/a/1190000012798495#articleHeader9](https://segmentfault.com/a/1190000012798495#articleHeader9)