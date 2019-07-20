---
layout: post
title:  "python中with和contextlib用法解析"
categories: python
tags:  python with contextlib
author: qmsggg37
---

* content
{:toc}

>用的os模块，读取文件的时候，其实他是含有__enter__ __exit__ 。  一个是with触发的时候，一个是退出的时候。
```
# -*- coding: utf-8 -*-
# @Time    : 2018/3/27 16:49
# @Author  : qmsggg
# @Email   : luohao0414@163.com
# @File    : With3.py
# @Software: PyCharm

if __name__ == '__main__':

    with file('/etc/hosts', 'r') as f:
        for line in f:
            print line

```

>那咱们自己再实现一个标准的可以with的类。 我个人写python的时候，喜欢针对一些需要有关闭逻辑的代码，构造成with的模式 。 
```
# -*- coding: utf-8 -*-
# @Time    : 2018/3/27 16:54
# @Author  : qmsggg
# @Email   : luohao0414@163.com
# @File    : MyWith.py
# @Software: PyCharm

class echo:
    def __enter__(self):
        print 'enter'

    def __exit__(self, *args):
        print 'exit'

if __name__ == '__main__':
    with echo() as e:
        print 'name'
```
**output**
```
enter
name
exit
```
>contextlib是个比with优美的东西，也是提供上下文机制的模块，它是通过Generator装饰器实现的，不再是采用__enter__和__exit__。contextlib中的contextmanager作为装饰器来提供一种针对函数级别的上下文管理机制。

```
# -*- coding: utf-8 -*-
# @Time    : 2018/3/27 16:57
# @Author  : qmsggg
# @Email   : luohao0414@163.com
# @File    : ContextmanagerMY.py
# @Software: PyCharm
from contextlib import contextmanager


@contextmanager
def make_context():
    print 'enter'
    try:
        yield {}
    except RuntimeError, err:
        print 'error', err
    finally:
        print 'exit'

if __name__ == '__main__':
    with make_context() as value:
        print value
```
**output**
```
enter
{}
exit

Process finished with exit code 0
```

