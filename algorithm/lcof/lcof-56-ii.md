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

### 2.2 方法二：哈希表--统计

> 计算出每一位1的个数，最后将总的个数%3得到答案。
>
> 注：效率慢

#### 代码实现

```text
/**
 * 统计
 */
public int singleNumber2(int[] nums) {
    int[] counts = new int[32];
    for (int num : nums) {
        for (int j = 0; j < 32; j++) {
            counts[j] += num & 1;
            num >>>= 1; //无符号位左移一位
        }
    }
    int res = 0, m = 3;
    for (int i = 0; i < 32; i++) {
        res <<= 1;
        res |= counts[31 - i] % m;
    }
    return res;
}
```

#### 复杂度分析

> 时间复杂度:O\(n\) \# O\(32n\)
>
> 空间复杂度:O\(1\) \# O\(32\)

## 3.参考

* [https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

