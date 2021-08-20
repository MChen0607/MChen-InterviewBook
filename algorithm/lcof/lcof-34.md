# 34. 二叉树中和为某一值的路径【DFS】

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
/**
 * DFS 回溯
 */
List<List<Integer>> res = new ArrayList<>();
List<Integer> path = new ArrayList<>();

public List<List<Integer>> pathSum(TreeNode root, int target) {
    recur(root, target);
    return res;
}

void recur(TreeNode root, int target) {
    if (root == null) {
        return;
    }
//        if (target < 0) {//剪枝 不能这么操作，本题没有设置结点的值大于0的限制。
//            return;
//        }
    path.add(root.val);
    target -= root.val;
    if (target == 0 && root.left == null && root.right == null)
        res.add(new ArrayList<>(path));
    recur(root.left, target);
    recur(root.right, target);
    // 回溯操作
    path.remove(path.size() - 1);
}
```

#### 复杂度分析

> 注：参考leetcode官方题解
>
> 时间复杂度：O\(N^2\)，其中 N是树的节点数。在最坏情况下，树的上半部分为链状，下半部分为完全二叉树，并且从根节点到每一个叶子节点的路径都符合题目要求。此时，路径的数目为 O\(N\)，并且每一条路径的节点个数也为 O\(N\)，因此要将这些路径全部添加进答案中，时间复杂度为 O\(N^2\)。
>
> 空间复杂度：O\(N\)，其中 N 是树的节点数。空间复杂度主要取决于栈空间的开销，栈中的元素个数不会超过树的节点数。

## 3.参考

* [https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)
* [https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/solution/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-68dg/](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/solution/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-68dg/)

