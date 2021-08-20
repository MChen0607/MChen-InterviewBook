# 32 -Ⅱ 从上到下打印二叉树Ⅱ【BFS/DFS】

## 1.题目描述s

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

### 2.1 方法一：BFS——队列

> BFS广度优先搜索，将每一层加入到队列中，计算队列的长度\[该层的结点数\]，循环遍历该长度的结点的，将值加入到list，并将结点的孩子结点加入到结点中。将该层次list加入到结果res二维数组中。总体循环至队列为空。

#### 代码实现

```text
/**
 * BFS 队列
 */
public List<List<Integer>> levelOrder2(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
        return res;
    }
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            list.add(node.val);
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }
        res.add(list);
    }
    return res;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) \# 辅助队列

### 2.2 方法二：DFS——递归

> DFS深度优先搜索递归实现，层级递归调用递归函数，递归函数包含层级，对应层级的结点加入到对应二维数组位置。

#### 代码实现

```text
/**
 * 递归
 */
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    dfs(res, root, 0);
    return res;
}

/**
 * @param res 二维数组
 * @param root 树
 * @param level 层级
 */
private void dfs(List<List<Integer>> res, TreeNode root, int level) {
    if (root == null) {
        return;
    }
    if (level >= res.size()) {
        res.add(new ArrayList<>());
    }
    res.get(level).add(root.val);
    dfs(res, root.left, level + 1);
    dfs(res, root.right, level + 1);
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) \# 递归栈

## 3.参考

* [https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

