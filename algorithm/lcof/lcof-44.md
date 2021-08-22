# 44. 数字序列中某一位的数字【数学】

## 1.题目描述

数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。

请写一个函数，求任意第n位对应的数字。

**示例 1：**

```text
 输入：n = 3
 输出：3
```

**示例 2：**

```text
 输入：n = 11
 输出：0
```

**限制：**

* `0 <= n < 2^31`

## 2.解题思路

### 2.1 方法一：数学

> 首先计算出n是几位数，进而确定是哪一个数，再确定该数的第几位。

#### 代码实现

```text
/**
 * 数学
 */
public int findNthDigit(int n) {
    int digit = 1;
    long start = 1;
    long count = 9;
    while (n > count) {
        n -= count;
        digit += 1;
        start *= 10;
        count = digit * start * 9;
    }
    long num = start + (n - 1) / digit;
    return Long.toString(num).charAt((n - 1) % digit) - '0';
}
```

#### 复杂度分析

> 时间复杂度：O\(log n\)
>
> 空间复杂度：O\(log n\)

## 3.参考

* [https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)
* [https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/solution/mian-shi-ti-44-shu-zi-xu-lie-zhong-mou-yi-wei-de-6/](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/solution/mian-shi-ti-44-shu-zi-xu-lie-zhong-mou-yi-wei-de-6/)

