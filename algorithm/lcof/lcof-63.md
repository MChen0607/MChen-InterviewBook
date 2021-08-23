# 63. 股票的最大利润【DP】

## 1.题目描述

假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

**示例 1:**

```text
 输入: [7,1,5,3,6,4]
 输出: 5
 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
      注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```

**示例 2:**

```text
 输入: [7,6,4,3,1]
 输出: 0
 解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**限制：**

* 0 &lt;= 数组长度 &lt;=10^5

## 2.解题思路

### 2.1 方法一：DP

> 使用minprice变量当前数组自己之前可购买的最小价格。
>
> maxprofit表示截止目前位置最大收益。

#### 代码实现

```text
/**
 * 动态规划
 */
public int maxProfit(int prices[]) {
    int minprice = Integer.MAX_VALUE;
    int maxprofit = 0;
    for (int price : prices) {
        if (price < minprice) {
            minprice = price;
        } else if (price - minprice > maxprofit) {
            maxprofit = price - minprice;
        }
    }
    return maxprofit;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

