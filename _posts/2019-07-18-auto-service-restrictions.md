---
layout: post
title:  "Auto类型推导"
categories: C++11
tags:  C++11 C++ coding 深入应用C++11  
author: qmsggg37
---

* content
{:toc}


## 前言

一直都只是简单的使用`C++11`的`auto关键字`，没有去系统的学习`auto关键字`的正确使用方式，
目前希望通过`深入应用C++11:代码优化与工程级应用`这本书系统的学些一下`auto`相关的用法。

## auto关键字的新意义
auto变量能自动根据初始化的值推导出变量类型；
不同与Python等动态类型语言的运行时变量类型推导，隐式类型定义的类型推导发生在编译期。
它的作用是让编译器自动推断出这个变量的类型，而不需要显示指定类型。
auto基本用法：
```
auto x = 5; // OK: x是int类型
auto pi = new auto(1); // OK: pi被推导为int*
const auto *v = &x, u = 6; // OK: v是const int*类型，u是const int类型
static auto y = 0.0； // OK: y是double类型
auto int r; // error: auto不再表示存储类型指示符
auto s; // error: auto无法推导出s的类型
```
