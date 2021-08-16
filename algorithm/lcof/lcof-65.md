# 65. 不用加减乘除做加法【位运算】

## 1.题目描述

写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“\*”、“/” 四则运算符号。

**示例:**

```text
 输入: a = 1, b = 1
 输出: 2
```

**提示：**

* `a`, `b` 均可能是负数或 0
* 结果不会溢出 32 位整数

## 2.解题思路

### 2.1 方法一：位运算

> 异或操作 0^1=1 1^0=1 0^0=0 1^1=0，那么先进行异或操作可以得出不考虑进位的情况 &与操作，可以得出哪一位发生进位，再将进位左移1位后，再与前面的异或操作的结果进行相加操作。直到无进位情况。

#### 代码实现

```text
 class Solution {
     public int add(int a, int b) {
         while (b != 0) {
             int c = (a & b) << 1;
             a = a ^ b;
             b = c;
         }
         return a;
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(1\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

