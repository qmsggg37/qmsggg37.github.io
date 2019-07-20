---
layout: post
title:  "已知二叉树的前序和中序，重建二叉树"
categories: structure
tags:  structure algorithm 二叉树 重建二叉树
author: qmsggg37
---

* content
{:toc}

题目如下：
输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
分析：

二叉树的前序遍历顺序是：先访问根节点，然后前序遍历左子树，再前序遍历右子树。

中序遍历顺序是：中序遍历根节点的左子树，然后是访问根节点，最后中序遍历右子树。

1、二叉树的前序遍历序列一定是该树的根节点

2、中序遍历序列中根节点前面一定是该树的左子树，后面是该树的右子树

从上面可知，题目中前序遍历的第一个节点{1}一定是这棵二叉树的根节点，根据中序遍历序列，可以发现中序遍历序列中节点{1}之前的{4,7,2}是这棵二叉树的左子树，{5,3,8,6}是这棵二叉树的右子树。然后，对于左子树，递归地把前序子序列{2,4,7}和中序子序列{4,7,2}看成新的前序遍历和中序遍历序列。此时，对于这两个序列，该子树的根节点是{2}，该子树的左子树为{4,7}、右子树为空，如此递归下去（即把当前子树当做树，又根据上述步骤分析）。{5,3,8,6}这棵右子树的分析也是这样。

代码如下：
```
class TreeNode {
	     int val;
	      TreeNode left;
	      TreeNode right;
	      TreeNode(int x) { val = x; }
	  }
public class TestRecoverBinaryTree {
	public TreeNode reConstructBinaryTree(int [] preOrder,int [] inOrder) 
	{
		int pLen = preOrder.length;
		int iLen = inOrder.length;
		if(pLen==0 && iLen==0)
        {
            return null;
        }
        return  btConstruct( preOrder, inOrder, 0, pLen-1,0, iLen-1);
    }
	//构建方法，pStart和pEnd分别是前序遍历序列数组的第一个元素和最后一个元素；
	//iStart和iEnd分别是中序遍历序列数组的第一个元素和最后一个元素。
	public TreeNode btConstruct(int[] preOrder, int[] inOrder, int pStart, int pEnd,int iStart,int iEnd)
	{
		//建立根节点
		TreeNode tree = new TreeNode(preOrder[pStart]);
		tree.left = null;
		tree.right = null;
		if(pStart == pEnd && iStart == iEnd)
		{
			return tree;
		}
		int root = 0;
		//找中序遍历中的根节点
		for(root=iStart; root<iEnd; root++)
		{
			if(preOrder[pStart] == inOrder[root])
			{
				break;
			}
		}
		//划分左右子树
		int leftLength = root - iStart;//左子树
		int rightLength = iEnd - root;//右子树
		//遍历左子树
		if(leftLength>0)
		{
			tree.left = btConstruct(preOrder, inOrder,  pStart+1,  pStart+leftLength, iStart, root-1);
		}
		//遍历右子树
		if(rightLength>0)
		{
			tree.right = btConstruct(preOrder, inOrder,  pStart+leftLength+1,  pEnd, root+1, iEnd);
		}
		return tree;
	}
}

```
注意：
已知前序和中序遍历，可以确定一棵二叉树。已知中序和后序遍历，可以确定一棵二叉树。但是，已知前序和后序遍历，不能确定一棵二叉树。
