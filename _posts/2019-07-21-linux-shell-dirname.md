---
layout: post
title:  "$ cd `dirname $0` shell变量的一些特殊用法"
categories: linux
tags:  linux shell dirname
author: qmsggg37
---

* content
{:toc}

在命令行状态下单纯执行` $ cd `dirname $0` `是毫无意义的。因为他返回当前路径的"."。

这个命令写在脚本文件里才有作用，他返回这个脚本文件放置的目录，

并可以根据这个目录来定位所要运行程序的相对位置（绝对位置除外）。

在`/home/admin/test/`下新建`test.sh`内容如下：

`cd `dirname $0``

`echo `pwd``

然后返回到`/home/admin/`执行

`sh test/test.sh`

运行结果:

`/home/admin/test`

这样就可以知道一些和脚本一起部署的文件的位置了，只要知道相对位置就可以根据这个目录来定位，

而可以不用关心绝对位置。这样脚本的可移植性就提高了，扔到任何一台服务器，（如果是部署脚本）都可以执行。
