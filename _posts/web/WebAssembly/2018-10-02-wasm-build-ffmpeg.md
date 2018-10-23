---
title: wasm 编译 ffmpeg
key: key-20181001
tags: wasm webAssembly emsdk c++
comment: true 
---

本文还未验证成功。
{:.error}

## 编译 ffmpeg 为 wasm 版本

我一开始以为难度会很大，后来发现并没有那么大，因为有一个 [videoconverter.js](https://bgrins.github.io/videoconverter.js/) 已经转过了（它是一个借助 ffmpeg 在网页实现音视频转码的），关键在于把一些没用的特性在 configure 的时候给disable掉，不然编译的时候会报语法错误。这里使用的是[emsdk](https://github.com/juj/emsdk)转的wasm，emsdk的安装方法在它的[安装教程](https://kripken.github.io/emscripten-site/docs/getting_started/downloads.html)已经说得很明白，主要是使用脚本判定系统下载不同编译好的文件。下载好之后就会有几个可执行文件，包括emcc、emc++、emar等命令，emcc是C的编译器，emc++是C++的编译器，而emar是用于把不同的.o库文件打包成一个.a文件的。

先要在[ffmpeg](https://www.ffmpeg.org/)的官网下载源码。

### 1. configure

解压进入目录，然后执行以下命令：

```bash
emconfigure ./configure --cc="emcc" 
--prefix=$(pwd)/../dist --enable-cross-compile --target-os=none --arch=x86_64 --cpu=generic --disable-ffplay --disable-ffprobe --disable-ffserver 
--disable-asm --disable-doc --disable-devices --disable-pthreads --disable-w32threads --disable-network --disable-hwaccels 
--disable-parsers --disable-bsfs --disable-debug --disable-protocols --disable-indevs --disable-outdevs --enable-protocol=file
```

执行报错：`Unknown option "--disable-ffserver"`，去掉 `Unknown option "--disable-ffserver"` 再执行。

通常configure的作用是生成Makefile——configure阶段确认一些编译的环境和参数，然后生成编译命令放到Makefile里面。

而前面的emconfigure的主要作用是把编译器指定为emcc，但只是这样是不够的，因为ffmpeg里面有一些子模块，并不能彻底地把所有的编译器都指定为emcc，好在ffmpeg的configure可以通过–cc的参数指定自定义的编译器，在Mac上C编译器一般是使用/usr/bin/clang，这里指定为emcc。

后面的disable是把一些不支持wasm的特性给禁掉了，例如–disable-asm是把使用汇编代码的部分给禁掉了，因为那些汇编语法emcc不兼容，没有禁掉的话编译会报错语法错误。另外一个–disable-hwaccels是把硬解码禁用了，有些显卡支持直接解码，不需要应用程序解码（软解码），硬解码性能明显会比软解码的高，

等待configure命令执行完了，就会生成Makefile和相关的一些配置文件。

### 2. make 

make是开始编译的阶段，执行以下命令进行编译：

```sh
emmake make
```
编译需要耗费
编译完成之后，会在ffmpeg目录生成一个总的ffmpeg文件，在ffmpeg的libavcodec等目录会生成libavcodec.a等文件，这些文件是后面我们要使用的bitcode文件，bitcode是一种已编译程序的中间代码。

最后在执行strip -o ffmpeg ffmpeg_g命令会挂掉，但是不要紧，strip改成cp ffmpeg_g ffmpeg就好了。
![mark](http://images.fuyix.cn/blog/181009/4LIjK8EDC9.png?imageslim)

这个ffmpeg文件就是我们第一步要得到的LLVM bitcode，下一步我们就可以将这个LLVM bitcode编译到js或者wasm里面啦

![mark](http://images.fuyix.cn/blog/181009/0i0lCG1g2J.png?imageslim)


```sh
cp ffmpeg-4.0.2/libavcodec/libavcodec.a ./lib2/
cp ffmpeg-4.0.2/libavformat/libavformat.a ./lib2/
cp ffmpeg-4.0.2/libavutil/libavutil.a ./lib2/
cp ffmpeg-4.0.2/libswresample/libswresample.a ./lib2/
cp ffmpeg-4.0.2/libswscale/libswscale.a ./lib2/

emcc libavcodec.a -o libavcodec.bc
emcc libavformat.a -o libavformat.bc
emcc libavutil.a -o libavutil.bc
emcc libswresample.a -o libswresample.bc
emcc libswscale.a -o libswscale.bc

# 下面这一步出错，暂时还没有找到解决办法。
emcc web.c process.c ../lib2/libavformat.bc ../lib2/libavcodec.bc ../lib2/libswscale.bc ../lib2/libswresample.bc ../lib2/libavutil.bc -Os -s WASM=1 -o index.html -s EXTRA_EXPORTED_RUNTIME_METHODS='["ccall", "cwrap"]' -s ALLOW_MEMORY_GROWTH=1 -s TOTAL_MEMORY=16777216
```

参考文献：

https://zhuanlan.zhihu.com/p/27910351

https://www.yinchengli.com/2018/07/28/wasm-ffmpeg-get-video-frame/?tdsourcetag=s_pctim_aiomsg

https://github.com/liyincheng/ffmpeg-wasm-video-to-picture

http://kripken.github.io/emscripten-site/docs/porting/emscripten-runtime-environment.html