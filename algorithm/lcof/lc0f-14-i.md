# 14 -Ⅰ 剪绳子【数学】

## 1.题目描述

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n&gt;1并且m&gt;1），每段绳子的长度记为 k\[0\],k\[1\]...k\[m-1\] 。请问 k\[0\]\*k\[1\]\*...\*k\[m-1\] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

**示例 1：**

> 输入: 2 输出: 1 解释: 2 = 1 + 1, 1 × 1 = 1

**示例 2:**

> 输入: 10 输出: 36 解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36

**提示：**

> **2 &lt;= n &lt;= 58**

## 2.解题思路

### 2.1 方法一：数学

> 通过数学公式求导推理可以得出，在分段的过程中，尽可能让每段都为3的情况下，乘积是最大的。
>
> 具体证明过程见参考链接。

#### 代码实现

```text
/**
 * 数学
 */
public int cuttingRope(int n) {
    if (n < 2) {
        return 0;
    }
    if (n <= 3) {
        return n - 1;
    }
    int times = n / 3;
    if (n - times * 3 == 1) {//当分完数为3的times段后，还剩1时，会发现 1 3的乘积小于2 2的乘积，所以此情况需要减少一次分为3的段。
        times--;
    }
    int mod = (n - times * 3) / 2;//当余数为0时 乘以1;余数为4时,乘以4;余数为2时乘以2.可转换为取2的（余数除以2）次方。
    return (int) Math.pow(3, times) * (int) Math.pow(2, mod);
}
```

#### 复杂性分析

> 时间复杂度: O\(1\)
>
> 空间复杂度：O\(1\)

## 3.参考

* **证明过程：**[https://leetcode-cn.com/problems/jian-sheng-zi-lcof/solution/mian-shi-ti-14-i-jian-sheng-zi-tan-xin-si-xiang-by/](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/solution/mian-shi-ti-14-i-jian-sheng-zi-tan-xin-si-xiang-by/)
* [https://leetcode-cn.com/problems/jian-sheng-zi-lcof/](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

