# 28. 对称的二叉树【递归】

## 1.题目描述

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 \[1,2,2,3,4,4,3\] 是对称的。

```text
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

  但是下面这个 \[1,2,2,null,3,null,3\] 则不是镜像对称的:

```text
    1
   / \
  2   2
   \   \
    3   3
```

 **示例 1：**

```text
 输入：root = [1,2,2,3,4,4,3]
 输出：true
```

**示例 2：**

```text
 输入：root = [1,2,2,null,3,null,3]
 输出：false
```

## 2.解题思路

### 2.1 方法一：递归

> 递归判断对称结点是否值相等

#### 代码实现

```text
/**
 * 递归
 */
public boolean isSymmetric(TreeNode root) {
    if (root == null) {
        return true;
    }
    return Symmetric(root.left, root.right);
}

public boolean Symmetric(TreeNode left, TreeNode right) {
    if (left == null && right == null) {
        return true;
    }
    if (left == null || right == null) {
        return false;
    }
    if (left.val != right.val) {
        return false;
    }
    return Symmetric(left.right, right.left) && Symmetric(left.left, right.right);
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) //递归调用栈使用。

## 3.参考

* [https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)
* 《剑指offer》

