---
layout: post
title:  "Python生成器对象的send方法"
categories: python
tags:  python send 生成器
author: qmsggg37
---

* content
{:toc}

>生成器对象是一个迭代器。但是它比迭代器对象多了一些方法，它们包括send方法，throw方法和close方法。这些方法，主要是用于外部与生成器对象的交互。本文先介绍send方法。
send方法有一个参数，该参数指定的是上一次被挂起的yield语句的返回值。这样说起来比较抽象，看下面的例子。
```
# -*- coding: utf-8 -*-

def MyGenerator():
    print 'b:'
    value = (yield 1)
    print 'value: %d' % value
    value = (yield value)
    print 'value2: %d' % value


gen = MyGenerator()
print gen.next()
print gen.send(2)
print gen.send(3)
```

>输出的结果如下
```
/Users/qmsggg/WorkSpace/Python/TheWayOfPython/pythonBaseStudy/venv/bin/python /Users/qmsggg/WorkSpace/Python/TheWayOfPython/pythonBaseStudy/micFunc/yielder.py
b:
Traceback (most recent call last):
1
value: 2
2
  File "/Users/qmsggg/WorkSpace/Python/TheWayOfPython/pythonBaseStudy/micFunc/yielder.py", line 14, in <module>
value2: 3
    print gen.send(3)
StopIteration

Process finished with exit code 1
```

>上面代码的运行过程如下。
当调用gen.next()方法时，python首先会执行MyGenerator方法的yield 1语句。由于是一个yield语句，因此方法的执行过程被挂起，而next方法返回值为yield关键字后面表达式的值，即为1。

>当调用gen.send(2)方法时，python首先恢复MyGenerator方法的运行环境。同时，将表达式(yield 1)的返回值定义为send方法参数的值，即为2。这样，接下来value=（yield 1）这一赋值语句会将value的值置为2。继续运行会遇到yield value语句。因此，MyGenerator方法再次被挂起。同时，send方法的返回值为yield关键字后面表达式的值，也即value的值，为2。

>当调用send(3)方法时MyGenerator方法的运行环境。同时，将表达式(yield value)的返回值定义为send方法参数的值，即为3。这样，接下来value=（yield value）这一赋值语句会将value的值置为3。继续运行，MyGenerator方法执行完毕，故而抛出StopIteration异常。



>总的来说，send方法和next方法唯一的区别是在执行send方法会首先把上一次挂起的yield语句的返回值通过参数设定，从而实现与生成器方法的交互。但是需要注意，在一个生成器对象没有执行next方法之前，由于没有yield语句被挂起，所以执行send方法会报错。例如
```
gen = MyGenerator()  
print gen.send(2) 
```

>上面代码的输出为
```
/Users/qmsggg/WorkSpace/Python/TheWayOfPython/pythonBaseStudy/venv/bin/python /Users/qmsggg/WorkSpace/Python/TheWayOfPython/pythonBaseStudy/micFunc/yielder.py
Traceback (most recent call last):
  File "/Users/qmsggg/WorkSpace/Python/TheWayOfPython/pythonBaseStudy/micFunc/yielder.py", line 13, in <module>
    print gen.send(2)
TypeError: can't send non-None value to a just-started generator

Process finished with exit code 1

```
>当然，下面的代码是可以接受的
```
gen = MyGenerator()  
print gen.send(None)  

```
>因为当send方法的参数为None时，它与next方法完全等价。但是注意，虽然上面的代码可以接受，但是不规范。所以，在调用send方法之前，还是先调用一次next方法为好。