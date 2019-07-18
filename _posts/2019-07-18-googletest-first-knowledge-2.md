---
layout: post
title:  "googletest使用系列之一:初识gtest"
categories: googletest
tags:  google gtest coding googletest  
author: qmsggg37
---

* content
{:toc}
## 前言
Google test是一种比较方便的C++测试框架， 它能够帮助我们比较方便的进行测试代码的编写, 以及输出尽可能详细的失败信息。能够大大缩短我们测试代码的编写效率， 而且该框架的使用方法也比较简单， 能够降低我们学习新框架的负担。

## 编译、安装
```
$ git clone https://github.com/google/googletest.git
$ cd googletest
$ mkdir mybuild
$ cd mybuild
$ cmake -Dgtest_build_tests=on -DCMAKE_INSTALL_PREFIX=. ..
$ make; make install
```
经过编译之后， 在mybuild目录下面， 产生了lib 和include目录， 有我们需要的头文件：include/gtest/gtest.h 以及静态库： lib/libgtest.a。
