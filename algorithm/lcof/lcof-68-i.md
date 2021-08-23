# 68-Ⅰ 二叉搜索树的最近公共祖先【二叉搜索树性质】

## 1.题目描述

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

例如，给定如下二叉搜索树: root = \[6,2,8,0,4,7,9,null,null,3,5\]

![](https://mchen0607.github.io/images/binarysearchtree_improved.png)

**示例 1:**

```text
 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
 输出: 6 
 解释: 节点 2 和节点 8 的最近公共祖先是 6。
```

**示例 2:**

```text
 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
 输出: 2
 解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```

**说明:**

* 所有节点的值都是唯一的。
* p、q 为不同节点且均存在于给定的二叉搜索树中。

## 2.解题思路

### 2.1 方法一：二叉搜索树性质

> 利用二叉搜索树的性质来进行本题解答，找出两个节点的最近公共祖先。二叉搜索树保证：左子树的所有结点的值小于根节点的值，右子树的所有结点值大于根节点的值。每个结点都满足这样的性质
>
> 那么我们可以从树的根节点来判断，如果两个结点的值都小于根节点的值，则这两个结点都在根结点的左子树中，那么继续遍历跟的左子树。
>
> 同理，如果两个结点的值都大于根节点的值，那么继续遍历跟的右子树。
>
> 如果出现其他情况，则可能为根节点就为其中两个结点的一个结点，或者两个结点的值分别大于、小于根节点的值，则根节点为最近公共祖先

#### 代码实现

```text
/**
 * 性质
 */
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    TreeNode ancestor = root;
    while (true) {
        if (p.val < ancestor.val && q.val < ancestor.val) {
            ancestor = ancestor.left;
        } else if (p.val > ancestor.val && q.val > ancestor.val) {
            ancestor = ancestor.right;
        } else {
            break;
        }
    }
   
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

