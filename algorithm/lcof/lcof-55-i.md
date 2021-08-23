# 55 -Ⅰ 二叉树的深度【DFS/层次遍历】

## 1.题目描述

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

例如：

给定二叉树 `[3,9,20,null,null,15,7]`，

```text
     3
    / \
   9  20
     /  \
    15   7
```

返回它的最大深度 3 。

**提示：**

1. `节点总数 <= 10000`

## 2.解题思路

### 2.1 方法一：深度优先搜索

> dfs方法，后序遍历，计算出左右子树的高度，递归。

#### 代码实现

```text
/**
 * DFS--定义函数
 */
public int maxDepth(TreeNode root) {
    if (root == null) {
        return 0;
    }
    return dfs(root);
}

private int dfs(TreeNode root) {
    if (root == null) {
        return 0;
    }
    int leftLevel = dfs(root.left);
    int rightLevel = dfs(root.right);
    return Math.max(leftLevel, rightLevel) + 1;
}

/**
 * DFS--递归自己的函数
 */
public int maxDepth2(TreeNode root) {
    if (root == null) {
        return 0;
    }
    return Math.max(maxDepth2(root.left), maxDepth2(root.right)) + 1;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) 递归栈

### 2.2 方法二：层次遍历

> 层次遍历，计算有多少层。

#### 代码实现

```text
/**
 * 层次遍历
 */
public int maxDepth3(TreeNode root) {
    if (root == null) {
        return 0;
    }
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    int res = 0;
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }
        res++;
    }
    return res;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

