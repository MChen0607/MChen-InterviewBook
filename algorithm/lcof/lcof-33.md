# 33. 二叉搜索树的后序遍历序列

## 1.题目描述

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

参考以下这颗二叉搜索树：

```text
      5
     / \
    2   6
   / \
  1   3
```

**示例 1：**

```text
 输入: [1,6,3,2,5]
 输出: false
```

**示例 2：**

```text
 输入: [1,3,2,6,5]
 输出: true
```

**提示：**

1. `数组长度 <= 1000`

## 2.解题思路

### 2.1 方法一：栈

### 2.2 方法二：递归？？？

```text
 class Solution33 {
     // 递归调用
     public boolean verifyPostorder(int[] postorder) {
         return recur(postorder, 0, postorder.length - 1);
     }
 ​
     boolean recur(int[] postorder, int i, int j) {
         if (i >= j) return true;
         int p = i;
         while (postorder[p] < postorder[j]) p++;//左子树部分
         int m = p;
         while (postorder[p] > postorder[j]) p++;//右子树部分
         // 正常的后序遍历  左子树|右子树|根结点，经过遍历查找左子树和右子树后p会等于j，否则该序列不为搜索二叉树的后序遍历序列
         return p == j && recur(postorder, i, m - 1) && recur(postorder, m, j - 1);
     }
 ​
     // 迭代：单调栈
     public boolean verifyPostorder2(int[] postorder) {
         Stack<Integer> stack = new Stack<>();
         int root = Integer.MAX_VALUE;
         for (int i = postorder.length - 1; i >= 0; i--) {
             if (postorder[i] > root) {
                 return false;
             }
             while (!stack.isEmpty() && stack.peek() > postorder[i]) {
                 root = stack.pop();
             }
             stack.push(postorder[i]);
         }
         return true;
     }
 }
```

## 3.参考

* [https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

