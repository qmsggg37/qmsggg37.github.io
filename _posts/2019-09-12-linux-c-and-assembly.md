---
layout: post
title:  "c程序和其对应的汇编程序"
date:   2019-09-12 21:0:54
categories: linux
tags: linux c assembly
author: qmsggg37
mathjax: true
---

* content
{:toc}

c程序和其对应的汇编程序

c语言程序:
```
int g(int x)
{
    return x + 3;
}

int f(int x)
{
    return g(x);
}

int main(void)
{
    return f(8) + 1;
}
```

使用汇编指令编译结果：
```
gcc -S -o linux_test.s linux_test.c -m32
注释：-m32是编译成32位

        .file   "linux_test.c"
        .text
        .globl  g
        .type   g, @function
g:
.LFB0:
        .cfi_startproc
        pushl   %ebp
        .cfi_def_cfa_offset 8
        .cfi_offset 5, -8
        movl    %esp, %ebp
        .cfi_def_cfa_register 5
        call    __x86.get_pc_thunk.ax
        addl    $_GLOBAL_OFFSET_TABLE_, %eax
        movl    8(%ebp), %eax
        addl    $3, %eax
        popl    %ebp
        .cfi_restore 5
        .cfi_def_cfa 4, 4
        ret
        .cfi_endproc
.LFE0:
        .size   g, .-g
        .globl  f
        .type   f, @function
f:
.LFB1:
        .cfi_startproc
        pushl   %ebp
        .cfi_def_cfa_offset 8
        .cfi_offset 5, -8
        movl    %esp, %ebp
        .cfi_def_cfa_register 5
        call    __x86.get_pc_thunk.ax
        addl    $_GLOBAL_OFFSET_TABLE_, %eax
        pushl   8(%ebp)
        call    g
        addl    $4, %esp
        leave
        .cfi_restore 5
        .cfi_def_cfa 4, 4
        ret
        .cfi_endproc
.LFE1:
        .size   f, .-f
        .globl  main
        .type   main, @function
main:
.LFB2:
        .cfi_startproc
        pushl   %ebp
        .cfi_def_cfa_offset 8
        .cfi_offset 5, -8
        movl    %esp, %ebp
        .cfi_def_cfa_register 5
        call    __x86.get_pc_thunk.ax
        addl    $_GLOBAL_OFFSET_TABLE_, %eax
        pushl   $8
        call    f
        addl    $4, %esp
        addl    $1, %eax
        leave
        .cfi_restore 5
        .cfi_def_cfa 4, 4
        ret
        .cfi_endproc
.LFE2:
        .size   main, .-main
        .section        .text.__x86.get_pc_thunk.ax,"axG",@progbits,__x86.get_pc_thunk.ax,comdat
        .globl  __x86.get_pc_thunk.ax
        .hidden __x86.get_pc_thunk.ax
        .type   __x86.get_pc_thunk.ax, @function
__x86.get_pc_thunk.ax:
.LFB3:
        .cfi_startproc
        movl    (%esp), %eax
        ret
        .cfi_endproc
.LFE3:
        .ident  "GCC: (Ubuntu 7.4.0-1ubuntu1~18.04.1) 7.4.0"
        .section        .note.GNU-stack,"",@progbits
        
注释：以点为前缀的都是链接的时候用的信息，这里可以把它删除了

g:
        pushl   %ebp
        movl    %esp, %ebp
        call    __x86.get_pc_thunk.ax
        addl    $_GLOBAL_OFFSET_TABLE_, %eax
        movl    8(%ebp), %eax
        addl    $3, %eax
        popl    %ebp
        ret
f:
        pushl   %ebp
        movl    %esp, %ebp
        call    __x86.get_pc_thunk.ax
        addl    $_GLOBAL_OFFSET_TABLE_, %eax
        pushl   8(%ebp)
        call    g
        addl    $4, %esp
        leave
        ret
main:
        pushl   %ebp
        movl    %esp, %ebp
        call    __x86.get_pc_thunk.ax
        addl    $_GLOBAL_OFFSET_TABLE_, %eax
        pushl   $8
        call    f
        addl    $4, %esp
        addl    $1, %eax
        leave
        ret
__x86.get_pc_thunk.ax:
        movl    (%esp), %eax
        ret
```

汇编代码解析：

eip：指向当前执行的指令
当前函数的堆栈：
ebp：指向当前堆栈的栈底
esp：指向当前堆栈的栈顶

eax：暂存数值

```
g:
        pushl   %ebp
        movl    %esp, %ebp
        movl    8(%ebp), %eax
        addl    $3, %eax
        popl    %ebp
        ret
f:
        pushl   %ebp
        movl    %esp, %ebp
        pushl   8(%ebp)
        call    g
        addl    $4, %esp
        leave
        ret
main:
        # 把ebp的值0压栈，并且同时esp的值被修改
        pushl   %ebp
        # 然后把esp的值赋给ebp，即esp和ebp都指向栈的相同位置
        movl    %esp, %ebp
        # 把参数8压入栈
        pushl   $8
        # 调用f函数，publish eip（eip指向call f的下一条指令），同时movel f eip；
        call    f
        addl    $4, %esp
        addl    $1, %eax
        leave
        ret
```


