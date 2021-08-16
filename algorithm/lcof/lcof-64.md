# 64. 求1+2+3+...+n 【递归】

## 1.题目描述

求 `1+2+...+n` ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

**示例 1：**

```text
 输入: n = 3
 输出: 6
```

**示例 2：**

```text
 输入: n = 9
 输出: 45
```

**限制：**

* `1 <= n <= 10000`

## 2.解题思路

### 2.1 方法一：递归

> 使用递归方法，利用&&逻辑运算符来坐终止条件，例如A&&B，当A为true时，才进行B操作，如果A为false则不进行操作。

#### 代码实现

```text
 class Solution {
     public int sumNums(int n) {
         boolean flag = n > 0 && (n += sumNums(n - 1)) > 0;
         return n;
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/qiu-12n-lcof/](https://leetcode-cn.com/problems/qiu-12n-lcof/)

