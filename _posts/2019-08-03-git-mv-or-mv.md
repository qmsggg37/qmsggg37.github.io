---
layout: post
title:  "git mv 与 mv"
date:   2019-08-03 09:32:54
categories: git
tags:  git git_mv mv
author: qmsggg37
---

* content
{:toc}

## 准备
创建并准备好git库，添加好文件：
```
root@mac /mac/test
$ git init testMV
Initialized empty Git repository in /mac/test//testMV/.git/

root@mac /mac/test
$ cd testMV/

root@mac /mac/test/testMV (master)
$ ls

root@mac /mac/test/testMV (master)
$ touch test.cpp

root@mac /mac/test/testMV (master)
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        test.cpp

nothing added to commit but untracked files present (use "git add" to track)

root@mac /mac/test/testMV (master)
$ git add .

root@mac /mac/test/testMV (master)
$ git commit -m "add test"
[master (root-commit) 7d083a0] add test
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.cpp

root@mac /mac/test/testMV (master)

```
## git mv
执行git mv流程：
1.创建一个和之前文件内容一样的文件，文件名为新的文件名
2.将原来的文件删除
3.将删除的文件添加到暂存区
4.将新建的文件添加到暂存区
```

root@mac /mac/test/testMV (master)
$ git mv test.cpp test2.cpp

root@mac /mac/test/testMV (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        renamed:    test.cpp -> test2.cpp


root@mac /mac/test/testMV (master)

```
如果要保存修改直接commit就可以了；
```
root@mac /mac/test/testMV (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        renamed:    test.cpp -> test2.cpp


root@mac /mac/test/testMV (master)
$ git commit -m "git mv test.cpp->test2.cpp"
[master 4dfdd04] git mv test.cpp->test2.cpp
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename test.cpp => test2.cpp (100%)

l00
```
## 系统的mv
```
root@mac /mac/test/testMV ((4dfdd04...))
$ mv test2.cpp test3.cpp

root@mac /mac/test/testMV ((4dfdd04...))
$ git status
HEAD detached at 4dfdd04
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    test2.cpp

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        test3.cpp

no changes added to commit (use "git add" and/or "git commit -a")
```
以上行为只是重新命名了一下文件名；
