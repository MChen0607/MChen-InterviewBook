# 56 -Ⅰ 数组中数字出现的次数【数学】

## 1.题目描述

一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是**O\(n\)**，空间复杂度是**O\(1\)**。

**示例 1：**

```text
 输入：nums = [4,1,4,6]
 输出：[1,6] 或 [6,1]
```

**示例 2：**

```text
 输入：nums = [1,2,10,4,1,4,3,3]
 输出：[2,10] 或 [10,2]
```

**限制：**

* `2 <= nums.length <= 10000`

## 2.解题思路

### 2.1 方法一：数学-异或

> 先遍历一遍数组，用0异或所有数，得出结果为t,则t为答案值a,b的异或结果，则将cp=1，进行处理，得到a,b两个数不同的最小位。
>
> 再一次遍历数组，如果数与cp的和&值为0，则被分为a，不为0,被分为b.
>
> 则结果a为a类所有数的异或值。

#### 代码实现

```text
 class Solution {
     public int[] singleNumbers(int[] nums) {
         int len = nums.length;
         if (len == 0) {
             return new int[0];
         }
         int a = 0, b = 0, t = 0;
         for (int i = 0; i < len; i++) {
             t = t ^ nums[i];
         }
         int cp = 1;
         while ((cp & t) == 0) {
             cp = cp << 1;
         }
         for (int i = 0; i < len; i++) {
             if ((cp & nums[i]) == 0) {
                 a = a ^ nums[i];
             }
         }
         return new int[]{a, a^t};//求出a后，则b=a^t  t为a^b.
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

