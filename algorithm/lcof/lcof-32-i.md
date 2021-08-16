# 32 -Ⅰ 从上到下打印二叉树

## 1.题目描述

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

例如: 给定二叉树: `[3,9,20,null,null,15,7]`,

```text
     3
    / \
   9  20
     /  \
    15   7
```

返回：

```text
 [3,9,20,15,7]
```

**提示：**

1. `节点总数 <= 1000`

## 2.解题思路

### 2.1 方法一：广度优先搜索、队列

```text
 public static class Solution32_1 {
     public int[] levelOrder(TreeNode root) {
         if (root == null) {
             return new int[0];
         }
         List<Integer> list = new LinkedList<>();
         Queue<TreeNode> queue = new LinkedList<>();
         queue.add(root);
         while (!queue.isEmpty()) {
             TreeNode node = queue.poll();
             list.add(node.val);
             if (node.left != null) {
                 queue.add(node.left);
             }
             if (node.right != null) {
                 queue.add(node.right);
             }
         }
         int[] res = new int[list.size()];
         for (int i = 0; i < list.size(); i++) {
             res[i] = list.get(i);
         }
         return res;
     }
 }
```

### 2.2 方法二：递归

## 3.参考

* [https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

