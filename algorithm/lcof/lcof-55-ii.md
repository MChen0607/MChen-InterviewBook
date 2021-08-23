# 55 -Ⅱ 平衡二叉树【DFS/递归】

## 1.题目描述

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

**示例 1:**

给定二叉树 `[3,9,20,null,null,15,7]`

```text
     3
    / \
   9  20
     /  \
    15   7
```

返回 `true` 。

**示例 2:**

给定二叉树 `[1,2,2,3,3,null,null,4,4]`

```text
        1
       / \
      2   2
     / \
    3   3
   / \
  4   4
```

返回 `false` 。

**限制：**

* `0 <= 树的结点个数 <= 10000`

## 2.解题思路

### 2.1 方法一：深度优先搜索

> dfs方法，后序遍历，计算出左右子树的长度，判断左右子树的高度差的绝对值是否小于1，即判断是否为平衡二叉树，递归。

#### 代码实现

```text
/**
 * DFS--后序遍历
 * -1为不平衡状态，平衡状态下返回树的高度
 */
public boolean isBalanced(TreeNode root) {
    return maxDepth(root) != -1;
}

private int maxDepth(TreeNode root) {
    if (root == null)
        return 0;
    int left = maxDepth(root.left);
    if (left == -1)
        return -1;
    int right = maxDepth(root.right);
    if (right == -1)
        return -1;
    return Math.abs(left - right) < 2 ? Math.max(left, right) + 1 : -1;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) 递归栈

### 2.2 方法二：递归

> 分治的方式进行判断数是否为平衡二叉树。
>
> 注：该方法复杂度高，计算量大且重复计算。

#### 代码实现

```text
/**
 * 递归
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) return true;
        return Math.abs(depth(root.left) - depth(root.right)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }

    private int depth(TreeNode root) {
        if (root == null) return 0;
        return Math.max(depth(root.left), depth(root.right)) + 1;
    }
}
```

#### 复杂度分析

> 时间复杂度：O\(n log n\)
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)
* [https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/solution/mian-shi-ti-55-ii-ping-heng-er-cha-shu-cong-di-zhi/](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/solution/mian-shi-ti-55-ii-ping-heng-er-cha-shu-cong-di-zhi/)

