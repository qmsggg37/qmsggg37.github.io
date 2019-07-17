---
layout: post
title:  "Git 免密码推送代码"
date:   2019-07-17 20:14:54
categories: git
tags: git tools
---

* content
{:toc}

本文我将讲述使用ssh生成秘钥，免密码向github推送代码。

## 核心步骤
* 第一步、在本地生成`id_rsa.pub`文件
* 第二步、将`id_rsa.pub`文件中的内容添加到`github`上的`Add SSH key`页面即可
     [https://github.com/settings/keys](https://github.com/settings/keys)
* 第三步、[必要]使用`ssh`地址而非`https`地址的方式`git clone`，或者改变`remote`远程`url`为`ssh`
   - 一种方式就是在`git clone`的时候使用`ssh`方式
   两个方式的`url`地址不同，认证方式也不同。使用`ssh`时保存密钥对以后可以不再输入帐号密码，
而`https`却不能。
   - 另一种方式就是改变`remote`远程`url`，如下：
    ```
    $ git remote rm origin  
    $ git remote add origin git@github.com:cld378632668/utils.git
    ```

## 详细步骤
以`mac`为例，其它操作系统也一样。
* 打开本地终端，执行 ssh-keygen 命令创建密钥对：
```
❯ ssh-keygen -t rsa -C  'qmsggg37@163.com'     
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/qmsggg/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/qmsggg/.ssh/id_rsa.
Your public key has been saved in /Users/qmsggg/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:UUnjU5gUBD8BCcrzqjWL2j8DE4W2WbidZkAs3b6d3Xw qmsggg37@163.com
The key's randomart image is:
+---[RSA 2048]----+
| +.+  ..oBO=.    |
|. B.+.  .+++     |
| o X+.  . =      |
|  = *o   . o     |
|   + o.oSo       |
|  o ..o . o E    |
|   o+      .     |
| . +oo           |
|..+.oo           |
+----[SHA256]-----+
```
说明：
```
-t 指定密钥类型，默认即 rsa ，可以省略
-C 设置注释文字，比如你的邮箱，可以省略
```

生成过程中会提示输入密码两次，如果不想在使用公钥的时候输入密码，可以回车跳过；
密钥默认保存位置在 `~/.ssh` 目录下，打开后会看到私钥文件 `id_rsa` 和公钥文件 `id_rsa.pub`；

* 复制公钥至`github`
[https://github.com/settings/keys](https://github.com/settings/keys)
 
## 扩展
本教程也适用于ssh免密码登录
