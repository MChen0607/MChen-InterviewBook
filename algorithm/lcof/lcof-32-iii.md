# 32 -Ⅲ 从上到下打印二叉树Ⅲ【BFS/递归】

## 1.题目描述

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

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
   [20,9],
   [15,7]
 ]
```

**提示：**

1. `节点总数 <= 1000`

## 2.解题思路

### 2.1 方法一：广度优先搜索——双端链表

> BFS方法加双端队列，对每个层级判断是奇数还是偶数，如果是奇数层，则从尾部插入；如果是偶数层，则从头部插入。将每层的链表（LinkedList）加入到二维数组res结果中返回。

#### 代码实现

```text
/**
 * BFS——双端链表
 * <p>
 * 多种方式，可以进行，只需对奇偶层逻辑进行处理后，对加入链表队列的方式有所不同就能达到目的效果
 */
public List<List<Integer>> levelOrder2(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
        return res;
    }
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    boolean isOdd = true; // 层级标记
    while (!queue.isEmpty()) {
        int size = queue.size();
        LinkedList<Integer> list = new LinkedList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            if (isOdd) {
                list.addLast(node.val);
            } else {
                list.addFirst(node.val);
            }
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }
        res.add(list);
        isOdd = !isOdd;
    }
    return res;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

### 2.2 方法二：递归——双端链表

#### 代码实现

```text
/**
 * 递归--双端链表
 */
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> a = new ArrayList<>();
    dfs(a, root, 0);
    return a;
}

public void dfs(List<List<Integer>> list, TreeNode root, int level) {
    if (root == null)
        return;
    if (level >= list.size()) {
        list.add(new LinkedList<>());
    }
    LinkedList<Integer> l = (LinkedList<Integer>) list.get(level);
    if (level % 2 == 1) {
        l.addFirst(root.val);
    } else {
        l.addLast(root.val);
    }
    dfs(list, root.left, level + 1);
    dfs(list, root.right, level + 1);
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

