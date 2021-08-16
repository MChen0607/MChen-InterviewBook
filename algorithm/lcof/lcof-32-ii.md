# 32 -Ⅱ 从上到下打印二叉树Ⅱ

## 1.题目描述

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

例如: 给定二叉树: `[3,9,20,null,null,15,7]`,

```text
     3
    / \
   9  20
     /  \
    15   7
```

返回其层次遍历结果：

```text
 [
   [3],
   [9,20],
   [15,7]
 ]
```

**提示：**

1. `节点总数 <= 1000`

## 2.解题思路

## 2.1 方法一：队列

## 2.2 方法二：递归

#### 代码实现

```text
 class Solution32_2 {
     public List<List<Integer>> levelOrder(TreeNode root) {
         List<List<Integer>> res = new ArrayList<>();
         level(res, root, 0);
         return res;
     }
 ​
     private void level(List<List<Integer>> res, TreeNode root, int level) {
         if (root == null) {
             return;
         }
         if (level >= res.size()) {
             res.add(new ArrayList<>());
         }
         res.get(level).add(root.val);
         level(res, root.left, level + 1);
         level(res, root.right, level + 1);
     }
 }
```

## 3.参考

* [https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

