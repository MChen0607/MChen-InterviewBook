# 56 -Ⅱ 数组中出现的次数Ⅱ【数学】

## 1.题目描述

在一个数组 `nums` 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

**示例 1：**

```text
 输入：nums = [3,4,3,3]
 输出：4
```

**示例 2：**

```text
 输入：nums = [9,1,7,9,7,9,7]
 输出：1
```

**限制：**

* `1 <= nums.length <= 10000`
* `1 <= nums[i] < 2^31`

## 2.解题思路

### 2.1 方法一：数学-状态机

> 数学方法 00-&gt;01-&gt;10-&gt;00;

#### **代码实现**

```text
 class Solution {
     public int singleNumber(int[] nums) {
         // two one
         int ones = 0, twos = 0;
         for(int num : nums){
             ones = ones ^ num & ~twos;
             twos = twos ^ num & ~ones;
         }
         return ones;
     }
 }
```

#### **复杂度分析**

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

