# 57. 和为s的两个数字【双指针】

## 1.题目描述

输入一个**递增排序**的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

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

### 2.2 方法二：哈希表

> 使用哈希表记录遍历过的数，确定target-num是否遍历过。
>
> 注：效率低

#### 代码实现

```text
/**
 * 哈希表
 * 适用范围：所有
 */
public int[] twoSum2(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int num : nums) {
        if (map.containsKey(target - num)) {
            return new int[]{num, target - num};
        } else {
            map.put(num, num);
        }
    }
    return new int[0];
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

