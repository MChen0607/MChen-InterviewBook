# 60. n个骰子的点数【DP】

## 1.题目描述

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。

**示例 1:**

```text
 输入: 1
 输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
```

**示例 2:**

```text
 输入: 2
 输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]
```

**限制：**

```text
 1 <= n <= 11
```

## 2.解题思路

### 2.1 方法一：动态规划

$$  
f\(n,x\)=\sum^{6}\_{1}f\(n-1,x-i\)✖\frac{1}{6}  
$$  


#### 代码实现

```text
 class Solution60 {
     public double[] dicesProbability(int n) {
         double[] dp = new double[6];
         Arrays.fill(dp, 1.0 / 6.0);
         for (int i = 2; i <= n; i++) {
             double[] tmp = new double[5 * i + 1];
             for (int j = 0; j < dp.length; j++) {
                 for (int k = 0; k < 6; k++) {
                     tmp[j + k] += dp[j] / 6.0;
                 }
             }
             dp = tmp;
         }
         return dp;
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n ^ 2\)
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)
* [https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/solution/jian-zhi-offer-60-n-ge-tou-zi-de-dian-sh-z36d/](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/solution/jian-zhi-offer-60-n-ge-tou-zi-de-dian-sh-z36d/)

