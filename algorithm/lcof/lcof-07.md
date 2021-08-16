# 07. 重建二叉树【递归/栈】

## 1.题目描述

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

**例如，给出**

> 前序遍历 preorder = \[3,9,20,15,7\] 中序遍历 inorder = \[9,3,15,20,7\] 返回如下的二叉树：

```text
     3
    / \
   9  20
     /  \
    15   7
```

**限制：**

> 0 &lt;= 节点个数 &lt;= 5000

## 2.解题思路

### 2.1 方法一：递归

> 根据前序和中序的关系进行遍历。
>
> 前序遍历的顺序：根结点，左孩子结点，右孩子结点
>
> 中序遍历的顺序：左孩子结点，根节点，右孩子结点
>
> 所有我们需要做的是：在中序遍历结果中找到根节点的所在位置，将中序遍历结果分为三部分（左，根，右）。
>
> 根据中序遍历结果的长度，进而可以把前序遍历结果也分为三个部分（根，左，右）。
>
> 递归调用，直到当前数组长度为0,返回结果。

#### 代码实现

```text
 public TreeNode buildTree(int[] preorder, int[] inorder) {
     int preleft = 0, preright = preorder.length - 1;
     int inleft = 0, inright = inorder.length - 1;
     return createTree(preorder, inorder, preleft, preright, inleft, inright);
 }
 ​
 TreeNode createTree(int[] preorder, int[] inorder, int preleft, int preright, int inleft, int inright) {
     if (preleft > preright || inleft > inright) {
         return null;
     }
     TreeNode root = new TreeNode(preorder[preleft]);
     int inIndex = inleft;
     for (int i = inleft; i <= inright; i++) {
         if (inorder[i] == preorder[preleft]) {
             inIndex = i;
         }
     }
     root.left = createTree(preorder, inorder, preleft + 1, preleft + inIndex - inleft, inleft, inIndex - 1);
     root.right = createTree(preorder, inorder, preleft + inIndex - inleft + 1, preright, inIndex + 1, inright);
     return root;
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) 递归调用为O\(h\),h为二叉树的高度、返回结果O\(n\).因为h&lt;n，所以总的空间复杂度为O\(n\).

### 2.2 方法二：递归+优化

> 在方法一的基础上，进行优化查找中序遍历中的根结点方法，利用map集合存储中序队列值与下标对应关系。

#### 代码实现

```text
 Map<Integer,Integer> map=new HashMap<>();
 public TreeNode buildTree2(int[] preorder, int[] inorder) {
     int preleft = 0, preright = preorder.length - 1;
     int inleft = 0, inright = inorder.length - 1;
     for(int i=0;i<inorder.length;i++){
         map.put(inorder[i],i);
     }
     return createTree2(preorder, inorder, preleft, preright, inleft, inright);
 }
 TreeNode createTree2(int[] preorder, int[] inorder, int preleft, int preright, int inleft, int inright) {
     if (preleft > preright || inleft > inright) {
         return null;
     }
     TreeNode root = new TreeNode(preorder[preleft]);
     int inIndex=map.get(preorder[preleft]);//优化查找方式。
     root.left = createTree(preorder, inorder, preleft + 1, preleft + inIndex - inleft, inleft, inIndex - 1);
     root.right = createTree(preorder, inorder, preleft + inIndex - inleft + 1, preright, inIndex + 1, inright);
     return root;
 }
 
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) 递归调用为O\(h\),h为二叉树的高度、返回结果O\(n\).因为h&lt;n，所以总的空间复杂度为O\(n\).

### 2.3 方法三：迭代

> 我们用一个栈和一个指针辅助进行二叉树的构造。初始时栈中存放了根节点（前序遍历的第一个节点），指针指向中序遍历的第一个节点；我们依次枚举前序遍历中除了第一个节点以外的每个节点。如果 index 恰好指向栈顶节点， 那么我们不断地弹出栈顶节点并向右移动 index，并将当前节点作为最后一个弹出的节点的右儿子； 如果 index 和栈顶节点不同，我们将当前节点作为栈顶节点的左儿子； 无论是哪一种情况，我们最后都将当前的节点入栈。

#### 代码实现：

```text
 public TreeNode buildTree3(int[] preorder, int[] inorder) {
     if (preorder.length == 0 || preorder.length == 0) {
         return null;
     }
     TreeNode root = new TreeNode(preorder[0]);
     Deque<TreeNode> stack = new LinkedList<TreeNode>();//辅助栈
     stack.push(root);
     int inorderIndex = 0;
     for (int i = 1; i < preorder.length; i++) {
         int preorderVal = preorder[i];
         TreeNode node = stack.peek();
         if (node.val != inorder[inorderIndex]) {//如果栈顶元素不为中序遍历指针的对应的值，那么当前的值就为栈顶元素的左孩子。
             node.left = new TreeNode(preorderVal);
             stack.push(node.left);
         } else {//如果相等，那么就将栈顶元素（当前根结点）出栈，右移中序遍历指针，循环判断，直到值不相等或者栈空。
             while (!stack.isEmpty() && stack.peek().val == inorder[inorderIndex]) {
                 node = stack.pop();
                 inorderIndex++;
             }
             //那么当前的值就为最后一个出栈的栈顶元素（当前根节点）的右孩子。
             node.right = new TreeNode(preorderVal);
             stack.push(node.right);
         }
     }
     return root;
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) 递归调用为O\(h\),h为二叉树的高度、返回结果O\(n\).因为h&lt;n，所以总的空间复杂度为O\(n\).

## 3.参考

* [https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof)
* [https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/solution/mian-shi-ti-07-zhong-jian-er-cha-shu-by-leetcode-s/](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/solution/mian-shi-ti-07-zhong-jian-er-cha-shu-by-leetcode-s/)

