# 43. 1到n整数中1出现的次数\*【数学】

## 1.题目描述

输入一个整数 `n` ，求1～n这n个整数的十进制表示中1出现的次数。

例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

**示例 1：**

```text
 输入：n = 12
 输出：5
```

**示例 2：**

```text
 输入：n = 13
 输出：6
```

**限制：**

* `1 <= n < 2^31`

## 2.解题思路

### 2.1 方法一：数学统计

> 分别计算每一位出现1的总数，再相加

#### 代码实现

```text
​/**
 * 数学
 */
public int countDigitOne(int n) {
    // mulk 表示 10^k
    long mulk = 1;
    int ans = 0;
    while (n >= mulk) {
        ans += (n / (mulk * 10)) * mulk + Math.min(Math.max(n % (mulk * 10) - mulk + 1, 0), mulk);
        mulk *= 10;
    }
    return ans;
}
```

## 3.参考

* [https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)
* [https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/solution/1n-zheng-shu-zhong-1-chu-xian-de-ci-shu-umaj8/](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/solution/1n-zheng-shu-zhong-1-chu-xian-de-ci-shu-umaj8/)

