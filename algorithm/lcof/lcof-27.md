# 27. 二叉树的镜像【递归、迭代】

## 1.题目描述

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

```text
      4
     / \
    2   7
   / \ / \
  1  3 6  9
  
```

 `4`

 `/ \`

 `2 7`

 `/ \ / \`

 `1 3 6 9` 镜像输出：

```text
      4
     / \
    7   2
   / \ / \
  9  6 3  1
```

**示例 1：**

```text
 输入：root = [4,2,7,1,3,6,9]
 输出：[4,7,2,9,6,3,1]
```

## 2.解题思路

### 2.1 方法一：递归

#### 代码实现

```text
 public TreeNode mirrorTree(TreeNode root) {
     if (root == null) {
         return null;
     }
     // 先记录下root的左右孩子结点，再进行递归调用
     TreeNode left = root.left;
     TreeNode right = root.right;
     root.left = mirrorTree(right);
     root.right = mirrorTree(left);
     return root;
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

### 2.2 方法二：迭代

> 将未交换的结点存入栈中，将交换的结点的孩子节点存入栈，使得每个结点都交换左右孩子结点即可。

#### 代码实现

```text
 public TreeNode mirrorTree2(TreeNode root) {
     if (root == null) {
         return null;
     }
     Stack<TreeNode> stack = new Stack<>();
     stack.add(root);
     while (!stack.isEmpty()) {
         TreeNode node = stack.pop();
         if (node.left != null) {
             stack.add(node.left);
         }
         if (node.right != null) {
             stack.add(node.right);
         }
         TreeNode temp = node.left;
         node.left = node.right;
         node.right = temp;
     }
     return root;
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)
* 《剑指offer》

