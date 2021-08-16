# 32 -Ⅲ 从上到下打印二叉树Ⅲ

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

### 2.1 方法一：队列、广度优先搜索

### 2.2 方法二：递归

代码实现

```text
 class Solution {
     /**
          * 递归
          */
     public List<List<Integer>> levelOrder(TreeNode root) {
         List<List<Integer>> res = new LinkedList<>();
         level(res, root, 0);
         return res;
     }
 ​
     private void level(List<List<Integer>> res, TreeNode root, int level) {
         if (root == null) {
             return;
         }
         if (level >= res.size()) {
             res.add(new LinkedList<>());
         }
         if ((level & 1) == 0) {
             res.get(level).add(root.val);
         } else {
             res.get(level).add(0, root.val);
         }
         level(res, root.left, level + 1);
         level(res, root.right, level + 1);
     }
 ​
     /**
          * 队列 迭代
          * <p>
          * 多种方式，可以进行，只需对奇偶层逻辑进行处理后，对加入链表队列的方式有所不同就能达到目的效果
          */
     public List<List<Integer>> levelOrder2(TreeNode root) {
         List<List<Integer>> res = new ArrayList<List<Integer>>();
         if (root == null) {
             return res;
         }
         Queue<TreeNode> queue = new LinkedList<>();
         int level = 1;
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
             if (level % 2 == 0) { //反转链表
                 Collections.reverse(list);
             }
             res.add(list);
             level++;
         }
         return res;
     }
 }
```

## 3.参考

* [https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

