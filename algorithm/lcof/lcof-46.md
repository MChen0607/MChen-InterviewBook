# 46. 把数字翻译成字符串【DP】

## 1.题目描述

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

**示例 1:**

```text
 输入: 12258
 输出: 5
 解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
```

**提示：**

* `0 <= num <`$$2^{31}$$ 

## 2.解题思路

### 2.1 方法一：动态规划

> 先将数字转化为字符串进行，进而判断连续的两个字符组成的两位字符串是否在10~25的范围内，分为两种情况进行动态规划。

动态规划的转移方程为：

分为两种情况：

* 第一种：第i位和第i-1位字符组成字符串**在** 10~25范围内，则

$$
dp[i]=dp[i-1]+dp[i-2]
$$

* 第二种：第i位和第i-1位字符组成字符串**不在** 10~25范围内，则

$$
dp[i]=dp[i-1]
$$

**此题可对数组优化为两个变量进行。**

#### 代码实现

```text
/**
 * DP
 */
public int translateNum(int num) {
    String s = String.valueOf(num);
    int a = 1, b = 1;
    for (int i = 1; i < s.length(); i++) {
        String temp = s.substring(i - 1, i + 1);
        int c = a;
        if (temp.compareTo("10") >= 0 && temp.compareTo("25") <= 0) {
            c = a + b;
        }
        b = a;
        a = c;
    }
    return a;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

