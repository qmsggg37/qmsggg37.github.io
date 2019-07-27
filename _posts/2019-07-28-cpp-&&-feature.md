---
layout: post
title:  "C++11 &&的特性"
categories: c++
tags:  c++11 &&
author: qmsggg37
---

* content
{:toc}

右值引用就是对一个右值进行引用的类型。因为右值不具名，所以我们只能通过引用的方式找到它。

无论声明左值引用还是右值引用都必须立即进行初始化，因为引用类型本身并不拥有所绑定对象的内存，
只是该对象的一个别名。通过右值引用的声明，该右值又“重获新生”，其生命周期与右值引用类型变量
的生命周期一样，只要该变量还活着，该右值临时变量将会一只存活下去。

看一下下面的代码：
----------------------------------------------------------------------------------
```
#include<iostream>
using namespace std;

int g_constructCount = 0;
int g_copyConstructCount = 0;
int g_destructCount = 0;

struct A
{
    A()
    {
        cout << "construct: " << ++g_constructCount << endl;
    }

    A(const A& a)
    {
        cout << "copy construct: " << ++g_copyConstructCount << endl;
    }

    ~A()
    {
        cout << "destuct: " << ++g_destructCount << endl;
    }
};

A GetA()
{
    return A();
}

int main()
{
    A a = GetA();
    return 0;
}

```
为了清楚地观察临时值，在GCC下编译时设置编译选项`-fno-elide-constructors`来关闭返回值优化效果

g++ rvalueReference.cpp --std=c++11 -fno-elide-constructors

输出结果：
```
construct: 1
copy construct: 1
destuct: 1
copy construct: 2
destuct: 2
destuct: 3
```
--------------------------------------------------------------------------------

从上面的例子中可以看到，在没有返回值优化的情况下，拷贝构造函数调用了两次，一次是Get()函数内部创建的
对象返回后构造一个临时对象产生的，另一次是在mian函数中构造a对象产生的。第二次的destruct是因为临时
对象在构造a对象之后就销毁了。如果开启返回值优化，输出结果将是：
g++ rvalueReference.cpp --std=c++11
```
construct: 1
destuct: 1
```
