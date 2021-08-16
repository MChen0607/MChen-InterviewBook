# 39. 数组中出现次数超过一半的数字

## 1.题目描述

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1:**

```text
 输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
 输出: 2
```

**限制：**

```text
 1 <= 数组长度 <= 50000
```

## 2.解题思路

### 2.1 方法一：排序

### 2.2 方法二：投票法

#### 代码实现

```text
 class Solution39 {
     // 排序 数组一定存在，则中间的数就是超过一半的数
     public int majorityElement(int[] nums) {
         Arrays.sort(nums);
         return nums[nums.length / 2];
     }
 ​
     // 投票法
     public int majorityElement2(int[] nums) {
         int ans = 0, vote = 0;
         for (int num : nums) {
             if (vote == 0) {
                 ans = num;
             }
             if (ans == num) {
                 vote++;
             } else {
                 vote--;
             }
         }
         return ans;
     }
 }
```

## 3.参考

* [https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

