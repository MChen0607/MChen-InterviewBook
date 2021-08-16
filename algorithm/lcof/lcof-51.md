# 51. 数组中的逆序对【分治、归并】

## 1.题目描述

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

**示例 1:**

```text
 输入: [7,5,6,4]
 输出: 5
```

**限制：**

```text
 0 <= 数组长度 <= 50000
```

## 2.解题思路

### 2.1 方法一：归并排序

> 利用分治算法思想，归并排序，在归并的过程中计算逆序对的大小；
>
> 具体思路讲解可前往leetcode网站上观看视频讲解，了解已在资料中给出。

#### 代码实现

```text
 class Solution {
     public int reversePairs(int[] nums) {
         if (nums == null || nums.length <= 1) {
             return 0;
         }
         int temp[] = new int[nums.length];
         return mergeSort(nums, 0, nums.length - 1, temp);
     }
 ​
     private int mergeSort(int[] nums, int left, int right, int[] temp) {
         if (left == right) {
             return 0;
         }
         int mid = left + (right - left) / 2;
         int leftPairs = mergeSort(nums, left, mid, temp);
         int rightPairs = mergeSort(nums, mid + 1, right, temp);
 ​
         if (nums[mid] <= nums[mid + 1]) {
             return leftPairs + rightPairs;
         }
 ​
         int mergePairs = merge(nums, left, mid, right, temp);
         return leftPairs + rightPairs + mergePairs;
     }
 ​
     private int merge(int[] nums, int left, int mid, int right, int[] temp) {
         int i = left, j = mid + 1;
         int count = 0;
         for (int k = left; k <= right; k++) {
             if (i == mid + 1) {
                 temp[k] = nums[j++];
             } else if (j == right + 1) {
                 temp[k] = nums[i++];
             } else if (nums[i] <= nums[j]) {
                 temp[k] = nums[i++];
             } else {
                 temp[k] = nums[j++];
                 count += mid - i + 1;
             }
         }
         for (int k = left; k <= right; k++) {
             nums[k] = temp[k];
         }
         return count;
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n log n \)
>
> 空间复杂度：O\(1\)

### 2.2 方法二：暴力法

> 两重循环遍历判断是否存在逆序对

#### 代码实现

```text
 public class Solution {
 ​
     public int reversePairs(int[] nums) {
         int count = 0;
         int len = nums.length;
         for (int i = 0; i < len - 1; i++) {
             for (int j = i + 1; j < len; j++) {
                 if (nums[i] > nums[j]) {
                     count++;
                 }
             }
         }
         return count;
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n^2 \)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)
* 视频讲解：[https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/solution/shu-zu-zhong-de-ni-xu-dui-by-leetcode-solution/](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/solution/shu-zu-zhong-de-ni-xu-dui-by-leetcode-solution/)

