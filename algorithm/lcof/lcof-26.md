# 26. 树的子结构【迭代/递归】

## 1.题目描述

输入两棵二叉树A和B，判断B是不是A的子结构。\(约定空树不是任意一个树的子结构\)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如: 给定的树 A:

```text
     3
    / \
   4   5
  / \
 1  2 
```

 给定的树 B：

```text
  4
 /
1 
```

返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

**示例 1：**

```text
 输入：A = [1,2,3], B = [3,1]
 输出：false
```

**示例 2：**

```text
 输入：A = [3,4,5,1,2], B = [4,1]
 输出：true
```

**限制：**

```text
 0 <= 节点个数 <= 10000
```

## 2.解题思路

### 2.1 方法一：迭代

> **步骤一**：首先在A中找到等于B头结点值的结点，如果存在，则进行继续判断子结点是否全部一致；
>
> **步骤二**：如果前面判断结果为false,则进行对当前结点的左右孩子进行搜索，搜索步骤同“**步骤一**”一致。

#### 代码实现

```text
/**
 * 迭代
 */
public boolean isSubStructure(TreeNode A, TreeNode B) {
    if (B == null || A == null) {
        return false;
    }
    boolean res = false;
    if (A.val == B.val) {//A的当前结点为B的结点，则进行向下判断子结点是否一致
        res = subStructure(A.left, B.left) && subStructure(A.right, B.right);
    }
    if (!res) {// 如果A的当前结点不包含B的树结构，则进行搜索A的左右孩子结点
        res = isSubStructure(A.left, B) || isSubStructure(A.right, B);
    }
    return res;
}

public boolean subStructure(TreeNode A, TreeNode B) {
    if (B == null) {
        return true;
    }
    if (A == null) { //B不为null
        return false;
    }
    if (A.val != B.val) {
        return false;
    }
    return subStructure(A.left, B.left) && subStructure(A.right, B.right);

}
```

#### 复杂度分析

> 时间复杂度：O\(MN\) //其中 M,N分别为树 A 和 树 B的节点数量 
>
> 空间复杂度：O\(M\) //树A的深度高度

### 2.2 方法二：递归

> 通过前序遍历方式进行递归判断是否为树的子结构

#### 代码实现

```text
/**
 * 递归
 */
public boolean isSubStructure2(TreeNode A, TreeNode B) {
    return (A != null && B != null) && (recur(A, B) || isSubStructure2(A.left, B) || isSubStructure2(A.right, B));
}

boolean recur(TreeNode A, TreeNode B) {
    if (B == null) {
        return true;
    }
    if (A == null || A.val != B.val) {
        return false;
    }
    return recur(A.left, B.left) && recur(A.right, B.right);
}
```

#### 复杂度分析

> 时间复杂度：O\(MN\) //其中 M,N分别为树 A 和 树 B的节点数量 
>
> 空间复杂度：O\(M\) //树A的深度高度

## 3.参考

* [https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)
* 复杂度分析：[https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/solution/mian-shi-ti-26-shu-de-zi-jie-gou-xian-xu-bian-li-p/](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/solution/mian-shi-ti-26-shu-de-zi-jie-gou-xian-xu-bian-li-p/)
* [https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/solution/mian-shi-ti-26-shu-de-zi-jie-gou-xian-xu-bian-li-p/](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/solution/mian-shi-ti-26-shu-de-zi-jie-gou-xian-xu-bian-li-p/)

