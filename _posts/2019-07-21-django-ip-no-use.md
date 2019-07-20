---
layout: post
title:  "django搭建的站点,通过ip不能访问"
categories: django
tags:  python django
author: qmsggg37
---

* content
{:toc}

## 问题：django搭建的站点，通过localhost和127.0.0.1能访问，但是通过ip不能访问
解决方法：启动服务时ip使用0.0.0.0
使用runserver启动
```
 python3 manage.py runserver 0.0.0.0:8089
```
