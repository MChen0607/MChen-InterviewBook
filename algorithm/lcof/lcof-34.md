# 34. 二叉树中和为某一值的路径

## 1.题目描述

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

**示例:** 给定如下二叉树，以及目标和 `target = 22`，

```text
               5
              / \
             4   8
            /   / \
           11  13  4
          /  \    / \
         7    2  5   1
```

返回:

```text
 [
    [5,4,11,2],
    [5,8,4,5]
 ]
```

**提示：**

1. `节点总数 <= 10000`

## 2.解题思路

### 2.1 方法一：深度优先搜索

#### 代码实现

```text
 class Solution34 {
     List<List<Integer>> res = new ArrayList<>();
     List<Integer> path = new ArrayList<>();
 ​
     public List<List<Integer>> pathSum(TreeNode root, int target) {
         recur(root, target);
         return res;
     }
 ​
     void recur(TreeNode root, int target) {
         if (root == null) return;
         //            if (target < 0) {//剪枝 不能这么操作，本题没有设置结点的值大于0的限制。
         //                return;
         //            }
         path.add(root.val);
         target -= root.val;
         if (target == 0 && root.left == null && root.right == null)
             res.add(new ArrayList<>(path));
         recur(root.left, target);
         recur(root.right, target);
         path.remove(path.size() - 1);
     }
 }
```

## 3.参考

* [https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

