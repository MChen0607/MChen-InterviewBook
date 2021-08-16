# 53 -Ⅱ 0到n-1中缺失的数字【二分查找、数学】

## 1.题目描述

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

**示例 1:**

```text
 输入: [0,1,3]
 输出: 2
```

**示例 2:**

```text
 输入: [0,1,2,3,4,5,6,7,9]
 输出: 8
```

**限制：**

```text
 1 <= 数组长度 <= 10000
```

## 2.解题思路

### 2.1 方法一：二分查找

> **仅**适用于数组**有序**。
>
> 在数组有序的情况下，可进行二分查找，当nums\[i\]==i时，表示前i位都是在正确的位置上，没有缺失,则left=mid+1;
>
> 而当nums\[i\]!=i时，意味着缺失的数字发生在i位置及前面，则让right=mid-1;
>
> 返回值为 left.

#### **代码实现**

```text
 // 数组有序的情况下
 public int missingNumber(int[] nums) {
     int left = 0, right = nums.length;
     while (left <= right) {
         int mid = left + (right - left) / 2;
         if (nums[mid] == mid) {
             left = mid + 1;
         } else {
             right = mid - 1;
         }
     }
     return left;
 }
```

#### **复杂度分析**

> 时间复杂度：O\(log n\)
>
> 空间复杂度：O\(1\)

### 2.2 方法二：数学

> 可适用于数组**有序**以及**乱序**
>
> 计算数组的和，用1-n之和减去数组的和，即为缺失的数字。

**代码实现**

```text
 public int missingNumber1(int[] nums) {
     int len = nums.length;
     int sum = 0;
     for (int num : nums) {
         sum += num;
     }
     return (len+1)*len/2-sum;
 }
 // 可能存在的问题，int溢出。
```

**复杂度分析**

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

