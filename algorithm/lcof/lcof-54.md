# 54. 二叉搜索树的第k大节点【DFS】

## 1.题目描述

给定一棵二叉搜索树，请找出其中第k大的节点。

**示例 1:**

```text
 输入: root = [3,1,4,null,2], k = 1
    3
   / \
  1   4
   \
    2
 输出: 4
```

**示例 2:**

```text
 输入: root = [5,3,6,2,4,null,null,1], k = 3
        5
       / \
      3   6
     / \
    2   4
   /
  1
 输出: 4
```

**限制：**

1 ≤ k ≤ 二叉搜索树元素个数

## 2.解题思路

### 2.1 方法一:深度优先搜索DFS

> 类似中序遍历思想，先遍历右子树后遍历左子树的，访问根节点时，对k进行减一操作，当k==0时，退出递归，设置res.

#### 实现代码

```text
 class Solution54 {
     int res = -1, n;
 ​
     public int kthLargest(TreeNode root, int k) {
         if (root == null || k == 0) {
             return -1;
         }
         n = k;
         dfs(root);
         return res;
     }
 ​
     private void dfs(TreeNode root) {
         if (root == null)
             return;
         dfs(root.right);
         if (n == 0) {
             return;
         }
         if (--n == 0) {
             res = root.val;
             return;
         }
         dfs(root.left);
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) // 递归栈空间

## 3.参考

* [https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

