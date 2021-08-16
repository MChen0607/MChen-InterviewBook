# 53-Ⅰ 在排序数组中查找数字Ⅰ【二分查找】

## 1.题目描述

统计一个数字在排序数组中出现的次数。

**示例 1:**

```text
 输入: nums = [5,7,7,8,8,10], target = 8
 输出: 2
```

**示例 2:**

```text
 输入: nums = [5,7,7,8,8,10], target = 6
 输出: 0
```

**限制：**

```text
 0 <= 数组长度 <= 50000
```

## 2.解题思路

### 2.1方法一：二分查找

> 先二分查找到target后，在对查找到的数字前后搜索，统计完所有target值的数量

#### **实现代码**

```text
 class Solution {
     public int search(int[] nums, int target) {
         int right = nums.length - 1, left = 0;
         while (left <= right) {
             int mid = left + (right - left) / 2;
             if (nums[mid] == target) {
                 int res = 1;
                 int temp = mid - 1;
                 mid++;
                 while (mid < nums.length && nums[mid] == target) {
                     res++;
                     mid++;
                 }
                 while (temp >= 0 && nums[temp] == target) {
                     res++;
                     temp--;
                 }
                 return res;
             } else if (nums[mid] < target) {
                 left = mid + 1;
             } else {
                 right = mid - 1;
             }
         }
         return 0;
     }
 }
```

**复杂度分析**

> 时间复杂度：O\(log n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

