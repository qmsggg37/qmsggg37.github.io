---
layout: post
title:  "回溯经典算法之八皇后问题"
categories: algorithm
tags:  algorithm 八皇后 回溯
author: qmsggg37
---

* content
{:toc}

## 问题分析
- 八皇后问题背景
- 盲目的枚举算法
- 加约束的枚举算法
- 回溯法及基本思想
- 回溯法应用
- 八皇后问题的递归回溯算法
- 八皇后问题的非递归回溯算法
### 背景
> 八皇后问题是一个以国际象棋为背景的问题： 如何能够在 8×8 的国际象棋棋盘上放置八个皇后，使得任何一个皇后都无法直接吃掉其他的皇后？为了达到此目的，任两个皇后都不能处于同一条横行、纵行或斜线上。
### 八皇后问题
>要在8*8的国际象棋棋盘中放8个皇后，使任意两个皇后都不能互相吃掉。规则：皇后能吃掉同一行、同一列、同一对角线的任意棋子。求所有的解。

**八皇后的两组解**
![image](https://user-images.githubusercontent.com/28669743/37871319-f8747338-301d-11e8-9d77-f85a9f6674ef.png)
### 问题分析
>设八个皇后为xi，分别在第i行(i=1，2，3，4……，8)；
问题的解状态：可以用(1,x1)，(2,x2)，……，(8,x8)表示8个皇后的位置；
由于行号固定，可简单记为：(x1,x2,x3,x4,x5,x6,x7,x8)；
问题的解空间：(x1,x2,x3,x4,x5,x6,x7,x8)，1≤xi≤8(i=1，2，3，4……，8)，共88个状态；
约束条件：八个(1,x1),(2,x2) ,(3,x3),(4,x4) ,(5,x5), (6,x6) , (7,x7), (8,x8)不在同一行、同一列和同一对角线上。

**原问题即：** 在解空间中寻找符合约束条件的解状态。
**按什么顺序去搜？**
- 目标是没有漏网之鱼，尽量速度快。
### 盲目的枚举算法
- 盲目的枚举算法
通过8重循环模拟搜索空间中的88个状态；
按枚举思想，以DFS的方式，从第1个皇后在第1列开始搜索，枚举出所有的“解状态”：
从中找出满足约束条件的“答案状态”。

**约束条件？**
1.按什么顺序去查找所有的解
  a.盲目的枚举算法  
```
void main()  {     
int x[100];     
for (x[1]=1;x[1]<=10;x[1]++)       
for (x[2]=1;x[2]<=10;x[2]++)        
 for (x[3]=1;x[3]<=10;x[3]++)          
 for (x[4]=1;x[4]<=10;x[4]++)            
 for (x[5]=1;x[5]<=10;x[5]++)               
for (x[6]=1;x[6]<=10;x[6]++)                
 for (x[7]=1;x[7]<=10;x[7]++)                  
 for (x[8]=1;x[8]<=10;x[8]++)                    
 if (check(x)==0)                        
 {                             
printf(x);                       
  }   }
```
2.该如何解决冲突的问题呢？
- 行；我们是按照行枚举的，保证了一行一个皇后；
- 列：判断是否存在x[i]=x[j]
- 对角线：主对角线的i-j与从对角线的i+j存在特殊关系，如图：
![image](https://user-images.githubusercontent.com/28669743/37871355-c54e1008-301e-11e8-9f04-e84a8c9807df.png)
**约束条件？**
- 不在同一列：xi≠xj；
- 不在同一主对角线上：xi-i ≠ xj-j；
- 不在同一负对角线上：xi+i ≠ xj+j。
- 违规的情况可以整合改写为：
abs(xi - xj)=abs( i-j )) or (xi = xj)
```
queen1( )
{
  int a[9];   for (a[1]=1;a[1]<=8;a[1]++)  　　　
    for (a[2]=1;a[2]<=8;a[2]++)  
　   for (a[3]=1;a[3]<=8;a[3]++)  　　　
	for (a[4]=1;a[4]<=8;a[4]++)
	   for (a[5]=1;a[5]<=8;a[5]++)
	        for (a[6]=1;a[6]<=8;a[6]++)  
	             for(a[7]=1;a[7]<=8;a[7]++)
		      for(a[8]=1;a[8]<=8;a[8]++){
			if (check(a,8)=0)   continue;
                                    else  
			      for(i=1;i<=8;i++)  
			            print(a[i]); 
		       }
}
```
```
check1(a[],n)
{int i,j;
 for(i=2;i<=n;i++) 　  for(j=1;j<=i-1;j++)  　     if (a[i]==a[j]) or
          (abs(a[i]-a[j])==abs(i-j))
	  return(0);
  return(1);  } 
```

### 回溯法

>有“通用的解题法”之称。
回溯法的基本做法是搜索，或是一种组织得井井有条的，能避免不必要搜索的穷举式搜索法。这种方法适用于解一些组合数相当大的问题。
回溯法在问题的解空间树中，按深度优先策略，从根结点出发搜索解空间树。算法搜索至解空间树的任意一点时，先判断该结点是否包含问题的解。如果肯定不包含，则跳过对该结点为根的子树的搜索，逐层向其祖先结点回溯；否则，进入该子树，继续按深度优先策略搜索。
**回溯法指导思想——走不通，就掉头。**
- 回溯法
>求问题所有解：要回溯到根，且根结点的所有子树都已被搜索遍才结束。
求任一解：只要搜索到问题的一个解就可结束
- 回溯算法设计过程
**step1** 确定问题的解空间；
**step2** 确定结点的扩展规则；
**step3** 搜索解空间。
- 回溯法应用-加约束的枚举算法
>如果能够排除那些没有前途的状态，会节约时间；
如(1,1, x3,x4,x5,x6,x7,x8)没有必要再扩展；
这种状态扩展后会产生86个结点；
同样的(1,2, x3,x4,x5,x6,x7,x8)，…也应被排除。
如何提前发现？
在每一次扩展E结点后，都进行检查；
对检查结果如何处理？
检查合格的才继续向下扩展；
遇到不合格的“掉头就走”。
只要当前枚举到的状态可行，就继续枚举下去。当找到一种方案或者无法继续枚举下去时，就退回到上一状态。退回到上一状态的过程叫做回溯，枚举下一个状态的过程叫做递归。
回溯就是像人走迷宫一样，先选择一个前进方向尝试，一步步试探，在遇到死胡同不能再往前的时候就会退到上一个分支点，另选一个方向尝试，而在前进和回撤的路上都设置一些标记，以便能够正确返回，直到达到目标或者所有的可行方案都已经尝试完为止。
![image](https://user-images.githubusercontent.com/28669743/37873800-33bc059a-3056-11e8-83c1-fe6e332cdad3.png)

- 回溯法应用-例1 b加约束的枚举算法
![image](https://user-images.githubusercontent.com/28669743/37873809-4f222184-3056-11e8-9ee6-35f149d465a9.png)
- 例1 b加约束的枚举算法
```
queen1( )
{int a[9];  for (a[1]=1;a[1]<=8;a[1]++)  　　　
   for (a[2]=1;a[2]<=8;a[2]++)  
    {if ( check(a,2)==0 )  continue;
　   for (a[3]=1;a[3]<=8;a[3]++)  　　　
        ……
	   {if (check(a,7)==0)   continue;
                    for(a[8]=1;a[8]<=8;a[8]++)
                       {if (check(a,8)==0)continue;
                             else  for(i=1;i<=8;i++)  
                                   print(a[i]); }
     } } } } } } }

check2 (int a[ ],int n) {//多次被调用，只是一重循环 
   int i;    for(i=1;i<=n-1;i++)       if (abs(a[i]-a[n])==abs(i-n)) 
        or (a[i]==a[n]) 　　  return(0);
   return(1); }
```
>此算法可读性很好，体现了“回溯”。但它只能解决八皇后问题，而不能解决任意的n皇后问题。
-  回溯法应用-算法说明
**八皇后问题中的核心代码：**
遍历过程函数；
check函数。
**解决此类问题的核心内容：**
解空间树的搜索算法；
估值/判断函数：判断哪些状态适合继续扩展，或者作为答案状态。

- 2 回溯法应用-n皇后问题
>介绍过的方法：
c递归回溯算法；
d非递归回溯算法；
**策略：能进则进，不能进则换，不能换则退**

-  回溯法应用-算法框架-递归算法框架
```
int a[n];
Queens(int k)
 {   if (k>n)    即表示最后一个皇后摆放完毕，输出结果;  else 
          for(i=下界 ; i<=上界; i++) //枚举K个皇后所有可能的路径        {依次从列顶端开始搜索，一直到列底端，直到找到合适位置,如果未找到，自动返回上层递归
               a[k]=i; 
            if (check(a,k))       //满足限界函数和约束条件，不冲突
            //递归摆放下一个皇后Queens (k+ 1);}
          }
 }
```
![image](https://user-images.githubusercontent.com/28669743/37873842-d97c91c0-3056-11e8-88e6-381e9fb7be81.png)

-  回溯法应用-算法框架-非递归回溯框架
```
int a[n],i;
初始化数组a[ ]；
i=1;
While (i>0(有路可走)) and (未达到目标) //还未回溯到头
 { if (i=n) 搜索到一个解，输出;  //搜索到叶结点
   else                     //正在处理第i个元素
      {a[i]第一个可能的值；
       while (a[i]不满足约束条件且在搜索空间内)   
           a[i]下一个可能的值；
       if (a[i]在搜索空间内)       
            {标识占用的资源; i=i+1;}    //扩展下一个结点
       else  {清理所占的状态空间；i=i-1;}  //回溯
     }
}
```

## C++
1.经典的回溯递归解法：
```
#include<stdio.h>
#include<iostream>
using namespace std;
//dfs，每行只能放一个元素，遍历每行的每个位置，用一个一维数组记录，最后检查是否符合要求
int ans;
int vis[10];
int abs(int x){
    return x > 0 ? x : -x;
}
bool check(int r,int c){
    for(int i = 1;i<r;i++){
        if(vis[i] == c) return false;
        if(vis[i] - c == r - i || vis[i] - c == i - r) return false;
    }
    return true;
}
void dfs(int r){
    if(r > 8){
        ans++;
        return;
    }
    for(int i = 1;i<=8;i++){
        if(check(r,i)){
            vis[r] = i;
            dfs(r+1);
            vis[r] = 0;
        }
    }
}
main(){
    dfs(1);
    cout<<ans<<endl;
}
```

2.对角线检查优化

```
/*用三个数组记录列，左对角线，右对角线信息，每次判断该位置是否符合要求，只在符合要求位置放置元素。*/
#include <iostream>
using namespace std;
const int maxn=105;
const int mo=100;
typedef long long ll;
int a[maxn],n = 8,ans=0;
bool b[maxn],c[maxn],d[maxn];
void sou(int x){
    if(x > n){
        ans++;
        return;
    }
    for(int i = 1;i <= n;i++)if(!(b[i] || c[x+i] || d[x-i+n])){
        b[i] =c [x+i]=d[x-i+n]=true;
        a[x] = i;
        sou(x+1);
        b[i] =c [x+i] = d[x-i+n]=false;
    }
}
int main(){
    sou(1);
    cout<<ans;
}
```

3.位运算：

```
//算法思想与上一相同，改用三个int来存储信息，采用位运算提取合适位置
#include<iostream>
#include<stdio.h>
using namespace std;
int board;
int n;
int ans = 0;
void n_queen(int col,int ld,int rd){
    if(col == board){
        ans++;
        return;
    }
    int pos = board & (~(col | ld | rd));
    while(pos){
        int p = pos & (-pos);
        pos = pos - p;
        n_queen(col | p , (ld | p) << 1,(rd | p) >> 1);
    }
}
int main(){
    cin>>n;
    board = (1 << n) - 1;
    n_queen(0,0,0);
    cout<<ans<<endl;
}
```

4.十行内的八皇后...

[https://www.zhihu.com/question/28543312](https://www.zhihu.com/question/28543312)


## python

```
    # -*- coding: utf-8 -*-
    # python默认为ascii编码，中文编码可以用utf-8
    import random
    # 随机模块
    def conflict(state, col):
        # 冲突函数，row为行，col为列
        row = len(state)
        for i in range(row):
            if abs(state[i] - col) in (0, row - i):  # 重要语句
                return True
        return False


    def queens(num=8, state=()):
        # 生成器函数
        for pos in range(num):
            if not conflict(state, pos):
                if len(state) == num - 1:
                    yield (pos,)
                else:
                    for result in queens(num, state + (pos,)):
                        yield (pos,) + result

    def queenprint(solution):
        # 打印函数
        def line(pos, length=len(solution)):
            return '. ' * (pos) + 'X ' + '. ' * (length - pos - 1)

        for pos in solution:
            print line(pos)
 
    for solution in list(queens(8)):
        print solution

    print '  total number is ' + str(len(list(queens())))
    print '  one of the range is:\n'
    queenprint(random.choice(list(queens())))
```
