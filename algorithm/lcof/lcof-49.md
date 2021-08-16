# 49. 丑数【双指针】

## 1.题目描述

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

**示例:**

```text
 输入: n = 10
 输出: 12
 解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```

**说明:**

1. `1` 是丑数。
2. `n` **不超过**1690。

## 2.解题思路

### 2.1 方法一：三指针

> 利用三个指针的方法，分别指向2，3，5的系数的位置，系数都为1开始，如果当前的数为系数于2，3，5的乘积，则系数右移一个数。

#### 实现代码

```text
 class Solution {
     public int nthUglyNumber(int n) {
         if(n<=0){
             return 0;
         }
         int[] dp=new int[n];
         dp[0]=1;
         int a=0;
         int b=0;
         int c=0;
         for(int i=1;i<n;i++){
             int num2=dp[a]*2;
             int num3=dp[b]*3;
             int num5=dp[c]*5;
             int num=Math.min(num2,Math.min(num3,num5));
             dp[i]=num;
             if(num==num2){
                 a++;
             }
             if(num==num3){
                 b++;
             }
             if(num==num5){
                 c++;
             }
         }
         return dp[n-1];
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/chou-shu-lcof/](https://leetcode-cn.com/problems/chou-shu-lcof/)

