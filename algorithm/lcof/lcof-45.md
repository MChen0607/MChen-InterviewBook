# 45. 把数组排成最小的数【排序】

## 1.题目描述

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

**示例 1:**

```text
 输入: [10,2]
 输出: "102"
```

**示例 2:**

```text
 输入: [3,30,34,5,9]
 输出: "3033459"
```

**提示:**

* `0 < nums.length <= 100`

**说明:**

* 输出结果可能非常大，所以你需要返回一个字符串而不是整数
* 拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0

## 2.解题思路

### 2.1 方法一：排序

> 将numbers数组先转换为String数组，进而对转换后的String 数组进行排序，排序的比较函数为 字符串小的排序在前即（x,y）-&gt;\(x+y\).compareTo\(y+x\)，最终将排序后的String数组依次合并形成最终答案。

#### 实现代码

```text
 import java.util.ArrayList;
 import java.util.Arrays;
 ​
 public class Solution {
     public String PrintMinNumber(int [] numbers) {
         int len = numbers.length;
         String[] strs = new String[len];
         for (int i = 0; i < len; i++) {
             strs[i] = String.valueOf(numbers[i]);
         }
         Arrays.sort(strs, (x, y) -> (x + y).compareTo(y + x));
         StringBuilder stringBuilder = new StringBuilder();
         for (int i = 0; i < len; i++) {
             stringBuilder.append(strs[i]);
         }
         return stringBuilder.toString();
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(log n\), \[排序O\(log n\)；其他O\(n\)\]
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

