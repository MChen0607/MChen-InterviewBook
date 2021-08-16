# 57. 和为s的两个数字【双指针】

## 1.题目描述

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

**示例 1：**

```text
 输入：nums = [2,7,11,15], target = 9
 输出：[2,7] 或者 [7,2]
```

**示例 2：**

```text
 输入：nums = [10,26,30,31,47,60], target = 40
 输出：[10,30] 或者 [30,10]
```

**限制：**

* `1 <= nums.length <= 10^5`
* `1 <= nums[i] <= 10^6`

&lt;!--more--&gt;

## 2.解题思路

### 2.1 方法一：双指针

> 从头尾两指针向中间遍历。

#### 代码实现

```text
 class Solution {
     public int[] twoSum(int[] nums, int target) {
         int left=0,right=nums.length-1;
         while(left<right){
             if(nums[left]+nums[right]<target){
                 left++;
             }else if(nums[left]+nums[right]>target){
                 right--;
             }else{
                 return new int[]{nums[left],nums[right]};
             }
         }
         return new int[2];
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

