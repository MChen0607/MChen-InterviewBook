# 15. 二进制中1的个数【位运算】

## 1.题目描述

请实现一个函数，输入一个整数（以二进制串形式），输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

**示例 1：**

> 输入：00000000000000000000000000001011 输出：3 解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。

**示例 2：**

> 输入：00000000000000000000000010000000 输出：1 解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。

**示例 3：**

> 输入：11111111111111111111111111111101 输出：31 解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。

**提示：**

> 输入必须是长度为 32 的 二进制串 。

## 2.解题思路

### 2.1 方法一:暴力

> 直接遍历,将n转为32位2进制，计算出1的个数

#### 代码实现

```text
/**
 * 暴力法
 */
public int hammingWeight(int n) {
    int res = 0;
    while (n != 0) {
        if ((n & 1) == 1)
            res++;
        n >>>= 1;
    }
    return res;
}
```

#### 复杂度分析

> 时间复杂度：O\(32\) 32位二进制
>
> 空间复杂度：O\(1\)

### 2.2 方法二：位运算

> 利用位运算的规律，n=n&\(n-1\)公式可以将n的二进制数中的最后一位1变为0; 比如 n=00011100 n-1=00011011 n&\(n-1\)=00011000.利用此公式可以减少循环的次数，降低为1的次数。 时间复杂度为 O\(m\) m为n中1的个数。

#### 代码实现

```text
/**
 * 位运算
 */
public int hammingWeight1(int n) {
    int res = 0;
    while (n != 0) {
        res++;
        n = n & (n - 1);
    }
    return res;
}
```

#### 复杂度分析

> 时间复杂度：O\(m\)   m&lt;=32
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof)

