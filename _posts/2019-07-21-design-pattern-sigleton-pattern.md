---
layout: post
title:  "Design Pattern Singleton"
categories: designPattern
tags:  designPattern Singleton
author: qmsggg37
---

* content
{:toc}

## 单例模式（`Singleton Pattern`）

>单例模式（`Singleton Pattern`）是一种常用的软件设计模式，该模式的主要目的是确保某一个类只有一个实例存在。
当你希望在整个系统中，某个类只能出现一个实例时，单例对象就能派上用场。

比如，某个服务器程序的配置信息存放在一个文件中，客户端通过一个 AppConfig 的类来读取配置文件的信息。
如果在程序运行期间，有很多地方都需要使用配置文件的内容，也就是说，很多地方都需要创建 AppConfig 对象的实例，
这就导致系统中存在多个 AppConfig 的实例对象，而这样会严重浪费内存资源，尤其是在配置文件内容很多的情况下。
事实上，类似 AppConfig 这样的类，我们希望在程序运行期间只存在一个实例对象。

## python

- 使用模块
>其实，Python 的模块就是天然的单例模式，因为模块在第一次导入时，会生成 .pyc 文件，当第二次导入时，就会直接加载 .pyc 文件，而不会再次执行模块代码。因此，我们只需把相关的函数和数据定义在一个模块中，就可以获得一个单例对象了。如果我们真的想要一个单例类，可以考虑这样做：
```
mysingleton.py

class Singleton(object):
    def foo(self):
        pass
singleton = Singleton()
```
>将上面的代码保存在文件 mysingleton.py 中，要使用时，直接在其他文件中导入此文件中的对象，这个对象即是单例模式的对象
```
from a import singleton
```
