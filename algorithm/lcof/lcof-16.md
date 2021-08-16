# 16. 数值的整数次方【快速幂】

## 1.题目描述

实现 pow\(x, n\) ，即计算 x 的 n 次幂函数（即，xn）。不得使用库函数，同时不需要考虑大数问题。

**示例 1：**

> 输入：x = 2.00000, n = 10 输出：1024.00000

**示例 2：**

> 输入：x = 2.10000, n = 3 输出：9.26100

**示例 3：**

> 输入：x = 2.00000, n = -2 输出：0.25000 解释：2-2 = 1/22 = 1/4 = 0.25

**提示：**【有问题】

* -100.0 &lt; x &lt; 100.0
* -231 &lt;= n &lt;= 231-1
* -104 &lt;= xn &lt;= 104

## 2.解题思路

### 2.1 方法一：快速幂

> 快速幂算法的核心思想就是每一步都把指数分成两半，而相应的底数做平方运算。这样不仅能把非常大的指数给不断变小，所需要执行的循环次数也变小，而最后表示的结果却一直不会变。
>
> 快速幂就是快速算底数的n次幂。其时间复杂度为 O\(log₂N\)。

#### 代码实现

```text
 static class Solution16 {
     public double myPow(double x, int n) {
         if(x == 0) return 0;
         long b = n;
         double res = 1.0;
         if(b < 0) {
             x = 1 / x;
             b = -b;
         }
         while(b > 0) {
             if((b & 1) == 1) res *= x;
             x *= x;
             b >>= 1;
         }
         return res;
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(log n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof)
