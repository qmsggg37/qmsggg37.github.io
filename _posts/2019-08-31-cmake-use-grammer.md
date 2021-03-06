---
layout: post
title:  "cmake常用变量和常用环境变量"
date:   2019-09-01 12:14:54
categories: cmake
tags: cmake 
author: qmsggg37
---

* content
{:toc}

## 一，cmake 变量引用的方式:
前面我们已经提到了，使用`${}`进行变量的引用。在 `IF` 等语句中，是直接使用变量名而不 通过`${}`取值
## 二，cmake 自定义变量的方式:
主要有`隐式`定义和`显式`定义两种，前面举了一个隐式定义的例子，就是 `PROJECT` 指令，他 会隐式的定义`<projectname>_BINARY_DIR` 
和`<projectname>_SOURCE_DIR` 两个变 量。
显式定义的例子我们前面也提到了，使用 `SET` 指令，就可以构建一个自定义变量了。 比如:
`SET(HELLO_SRC main.SOURCE_PATHc)`，就`PROJECT_BINARY_DIR`可以通过 `${HELLO_SRC}`来引用这个自定义变量了.




## 三，`cmake` 常用变量: 
  - 1，`CMAKE_BINARY_DIR` `PROJECT_BINARY_DIR` `<projectname>_BINARY_DIR`
    这三个变量指代的内容是一致的，如果是`in source`编译，指得就是工程顶层目录，
    如果 是 `out-of-source` 编译，指的是工程编译发生的目录。`PROJECT_BINARY_DIR` 跟其他 
    指令稍有区别，现在，你可以理解为他们是一致的。  
  - 2，`CMAKE_SOURCE_DIR` `PROJECT_SOURCE_DIR` `<projectname>_SOURCE_DIR`
    这三个变量指代的内容是一致的，不论采用何种编译方式，都是工程顶层目录。 也就是在`in source`编译时，他跟`CMAKE_BINARY_DIR`等变量一致。
    `PROJECT_SOURCE_DIR` 跟其他指令稍有区别，现在，你可以理解为他们是一致的。   
  - 3，`CMAKE_CURRENT_SOURCE_DIR`
  指的是当前处理的 `CMakeLists.txt` 所在的路径，比如上面我们提到的 `src` 子目录。   
  - 4，`CMAKE_CURRRENT_BINARY_DIR`
    如果是 `in-source` 编译，它跟 `CMAKE_CURRENT_SOURCE_DIR` 一致，如果是 `out-of- source` 编译，他指的是 `target` 编译目录。
    使用我们上面提到的`ADD_SUBDIRECTORY(src bin)`可以更改这个变量的值。  
    使用`SET(EXECUTABLE_OUTPUT_PATH <新路径>)`并不会对这个变量造成影响，它仅仅 修改了最终目标文件存放的路径。    
  - 5，`CMAKE_CURRENT_LIST_FILE` 输出调用这个变量的 `CMakeLists.txt` 的完整路径  
  - 6，`CMAKE_CURRENT_LIST_LINE` 输出这个变量所在的行  
  - 7，`CMAKE_MODULE_PATH`
    这个变量用来定义自己的 `cmake` 模块所在的路径。如果你的工程比较复杂，有可能会自己 编写一些 `cmake` 模块，
    这些 `cmake` 模块是随你的工程发布的，为了让 `cmake` 在处理 `CMakeLists.txt` 时找到这些模块，你需要通过 `SET` 指令，
    将自己的 `cmake` 模块路径设 置一下。
    比如`SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)` 这时候你就可以通过 `INCLUDE` 指令来调用自己的模块了。  
  - 8，`EXECUTABLE_OUTPUT_PATH` 和 `LIBRARY_OUTPUT_PATH` 分别用来重新定义最终结果的存放目录，前面我们已经提到了这两个变量。  
  - 9，`PROJECT_NAME`
    返回通过 `PROJECT`指令定义的项目名称。  
## 四，`cmake` 调用环境变量的方式 
使用`$ENV{NAME}`指令就可以调用系统的环境变量了。 比如
`MESSAGE(STATUS “HOME dir: $ENV{HOME}”) `设置环境变量的方式是:
`SET(ENV{变量名} 值)`

  - 1,`CMAKE_INCLUDE_CURRENT_DIR`
    自动添加 `CMAKE_CURRENT_BINARY_DIR` 和 `CMAKE_CURRENT_SOURCE_DIR` 到当前处理
    的 `CMakeLists.txt`。相当于在每个 `CMakeLists.txt` 加入: `INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR})`  
  - 2,`CMAKE_INCLUDE_DIRECTORIES_PROJECT_BEFORE`
    将工程提供的头文件目录始终至于系统头文件目录的前面，当你定义的头文件确实跟系统发
    生冲突时可以提供一些帮助。  
  - 3,`CMAKE_INCLUDE_PATH` 和 `CMAKE_LIBRARY_PATH` 我们在上一节已经提及。 

## 五，系统信息
  - 1,`CMAKE_MAJOR_VERSION`，`CMAKE 主版本号`，比如 `2.4.6` 中的 `2`   
  - 2,`CMAKE_MINOR_VERSION`，CMAKE` 次版本号，比如 `2.4.6` 中的 `4`   
  - 3,`CMAKE_PATCH_VERSION`，`CMAKE`补丁等级，比如`2.4.6` 中的`6`   
  - 4,`CMAKE_SYSTEM`，系统名称，比如 `Linux-2.6.22`   
  - 5,`CMAKE_SYSTEM_NAME`，不包含版本的系统名，比如 `Linux`   
  - 6,`CMAKE_SYSTEM_VERSION`，系统版本，比如 `2.6.22`   
  - 7,`CMAKE_SYSTEM_PROCESSOR`，处理器名称，比如 `i686`.   
  - 8,`UNIX`，在所有的类`UNIX`平台为`TRUE`，包括`OS X`和`cygwin 9`,`WIN32`，在所有的 `win32` 平台为 `TRUE`，包括 `cygwin`  

## 六，主要的开关选项:
  - 1，`CMAKE_ALLOW_LOOSE_LOOP_CONSTRUCTS`，用来控制`IF ELSE`语句的书写方式，在 下一节语法部分会讲到。  
  - 2，`BUILD_SHARED_LIBS`
      这个开关用来控制默认的库编译方式，如果不进行设置，使用 `ADD_LIBRARY` 并没有指定库
      类型的情况下，默认编译生成的库都是静态库。 如果`SET(BUILD_SHARED_LIBS ON)`后，默认生成的为动态库。   
  - 3，`CMAKE_C_FLAGS`
      设置 `C` 编译选项，也可以通过指令 `ADD_DEFINITIONS()`添加。  
  - 4，`CMAKE_CXX_FLAGS`
      设置 `C++`编译选项，也可以通过指令 `ADD_DEFINITIONS()`添加。  

小结:
绍了一些较常用的 `cmake` 变量，这些变量仅仅是所有 `cmake` 变量的很少一部分，目 前 `cmake` 的英文文档也是比较缺乏的，
如果需要了解更多的 `cmake` 变量，更好的方式是阅 读一些成功项目的 `cmake` 工程文件，比如 `KDE4` 的代码。
## 七，`cmake`常用指令
前面我们讲到了 `cmake` 常用的变量，相信“`cmake` 即编程”的感觉会越来越明显，无论如何， 我们仍然可以看到 `cmake` 比 `autotools` 要简单很多。
接下来我们就要集中的看一看`cmake` 所提供的常用指令。在前面的章节我们已经讨论了很多指令的用法，如 `PROJECT`，`ADD_EXECUTABLE`，`INSTALL`，
`ADD_SUBDIRECTORY`，`SUBDIRS`，`INCLUDE _DIRECTORIES`，`LINK_DIRECTORIES`，`TARGET_LINK_LIBRARIES`，`SET` 等。
接下来会引入更多的`cmake`指令，为了编写的方便，我们将按照`cmake man page`的顺序 来介绍各种指令，不再推荐使用的指令将不再介绍，
`INSTALL` 系列指令在安装部分已经做 了非常详细的说明，本节也不在提及。(你可以将本章理解成选择性翻译，但是会加入更多 的个人理解)
