---
layout: post
title:  "vector容器的大坑：对vector容器使用下标索引进行访问的时候程序崩溃！"
categories: c++
tags:  vector 下标索引 程序崩溃
author: qmsggg37
---

* content
{:toc}

程序中首次对一个vector对象进行下标索引赋值运算，即使该vector已经在构造函数中完成了初始化，
但是使用下标访问它，对它赋值仍然导致程序崩溃，提示访问了未初始化的对象或提示指针越界！

实际上，首次对空的vector进行增加元素，必须使用push_back()，如果对空的vector使用下标的方式来增加元素，
将会导致程序崩溃！
要么使用push_back()
要么在对空vector使用下标赋值之前先使用resize()函数修改vector容器的大小
以上限制仅仅针对于对空的vector使用下标操作时有效。
