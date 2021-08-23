# 62. 圆圈中最后剩下的数字\*【约瑟夫问题】

## 1.题目描述

0,1,···,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

**示例 1：**

```text
 输入: n = 5, m = 3
 输出: 3
```

**示例 2：**

```text
 输入: n = 10, m = 17
 输出: 2
```

**限制：**

* `1 <= n <= 10^5`
* `1 <= m <= 10^6`

## 2.解题思路

### \[约瑟夫问题\]

### 2.1 方法一：数学+递归

#### 递归方程

$$
f(n,m)=(f(n-1,m)+m) \% n
$$

#### 代码实现

```text
/**
 * 数学+递归
 */
public int lastRemaining(int n, int m) {
    return f(n, m);
}

private int f(int n, int m) {
    if (n == 1) {
        return 0;
    }
    int x = f(n - 1, m);
    return (m + x) % n;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) \#递归栈

### 2.2 方法二：数学+迭代

> 使用的迭代的方法优化递归。

#### 代码实现

```text
/**
 * 数学+迭代
 */
public int lastRemaining2(int n, int m) {
    int f = 0;
    for (int i = 2; i <= n; ++i) {
        f = (m + f) % i;
    }
    return f;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

### **2.3 问题**

本题是找出最后剩下的那个数，如何找到第n个淘汰的数呢？如何得出每一轮淘汰的数呢？

## 3.参考

* [https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)
* [https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-by-lee/](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-by-lee/)

