# 42. 连续子数组的最大和【DP】

## 1.题目描述

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O\(n\)。

**示例1:**

```text
 输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
 输出: 6
 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

**提示：**

* `1 <= arr.length <= 10^5`
* `-100 <= arr[i] <= 100`

## 2.解题思路

### 2.1 方法一：动态规划

> 遍历数组，如果当前的sum为正数，直接加上当前访问的数；如果为负数，则sum值重新赋值为当前访问的值。每次计算完与res结果进行比较，将最大值赋值给res。

#### 代码实现

```text
/**
 * DP
 */
public int maxSubArray(int[] nums) {
    int res = nums[0];
    int sum = 0;
    for (int num : nums) {
        if (sum > 0) {
            sum += num;
        } else {
            sum = num;
        }
        res = Math.max(res, sum);
    }
    return res;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

