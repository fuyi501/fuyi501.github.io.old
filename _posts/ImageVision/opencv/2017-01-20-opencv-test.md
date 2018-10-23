---
title: 测试 opencv 安装成功-cmake测试
key: 20170120
tags: opencv
comment: true
---

## 创建工作目录

```bash
mkdir ~/opencv-test
cd ~/opencv-test
vim DisplayImage.cpp
```

## 编辑如下代码

```cpp
#include <stdio.h>
#include <opencv2/opencv.hpp>
using namespace cv;
int main(int argc, char** argv )
{
	if ( argc != 2 )
	{
		printf("usage: DisplayImage.out <Image_Path>\n");
		return -1;
	}
	Mat image;
	image = imread( argv[1], 1 );
	if ( !image.data )
	{
		printf("No image data \n");
		return -1;
	}
	namedWindow("Display Image", WINDOW_AUTOSIZE );
	imshow("Display Image", image);
	waitKey(0);
	return 0;
}
```

## 创建CMake编译文件

```bash
vim CMakeLists.txt
```

写入如下内容

```makefile
cmake_minimum_required(VERSION 2.8)
project( DisplayImage )
find_package( OpenCV REQUIRED )
add_executable( DisplayImage DisplayImage.cpp )
target_link_libraries( DisplayImage ${OpenCV_LIBS} )
```

## 编译

```bash
cd ~/opencv-lena
cmake .
make
```

## 执行

此时文件夹中已经产生了可执行文件 `DisplayImage`，下载 lena.jpg 放在 opencv-lena 下，运行

```bash
./DisplayImage lena.jpg
```

# 测试 opencv 安装成功-make测试

## 创建文件及编写代码

新建一个文件夹 `opencv-make-test`

```bash

mkdir opencv-make-test && cd opencv-make-test
vim opencv_test.cpp
vim Makefile
```

opencv_test.cpp 中的代码如下：

```cpp
#include "cv.h"
#include "highgui.h"
#include <iostream>
using namespace std;
#define	PICTURE	"./01.jpg"
int main(void)
{
    IplImage* img = cvLoadImage(PICTURE, 0);
    cvNamedWindow( "test", 0 );
    cvShowImage("test", img);
    cvWaitKey(0);
    cvReleaseImage( &img );
    cvDestroyWindow( "test" );
    return 0;
}
```

Makefile 代码如下：

```makefile
CXX       = g++
CFLAGS    = -Wall 
LDFLAGS   = `pkg-config --cflags --libs opencv`

SRCS = $(wildcard *.cpp)
TARGETS = $(patsubst %.cpp, %, $(SRCS))

all:$(TARGETS)
$(TARGETS):$(SRCS)
	$(CXX) -o $@ $< $(LDFLAGS) $(CFLAGS)

clean:
	-rm -rf $(TARGETS) *~ .*swp

.PHONY: clean all
```

## 编译&运行

```bash
make
./opencv_test
```

显示效果是一张黑白图片，如下：

![](http://images.fuyix.cn/17-5-21/65283448-file_1495351469524_455d.png)
