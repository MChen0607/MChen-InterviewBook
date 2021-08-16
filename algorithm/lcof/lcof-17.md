# 17. 打印从1到最大的n位数

## 1.题目描述

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

**示例 1:**

> 输入: n = 1 输出: \[1,2,3,4,5,6,7,8,9\]

**说明：**

* 用返回一个整数列表来代替打印
* n 为正整数

## 2.解题思路

### 2.1 方法一：暴力法

> 直接声明相对应的空间的数组，通过对值直接+1的方式赋值。

#### 代码

```text
 public int[] printNumbers(int n) {
     int len = (int) Math.pow(10, n) - 1;
     int[] num = new int[len];
     for (int i = 0; i < len; i++) {
         num[i] = i + 1;
     }
     return num;
 }
```

#### 复杂度分析

> 时间复杂度: O\(10^n\)
>
> 空间复杂度: O\(10^n\)

#### 缺点分析

> 此**暴力法**可以处理**n较小**的情况，当n较大时，此方法的性能就比较差，需要使用其他的方法来进行对N较大时求解。

### 2.2 方法二：有待完善

## 3.资源

* [https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof)

