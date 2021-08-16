# 57 -Ⅱ 和为s的连续正数序列【双指针】

## 1.题目描述

输入一个正整数 `target` ，输出所有和为 `target` 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

**示例 1：**

```text
 输入：target = 9
 输出：[[2,3,4],[4,5]]
```

**示例 2：**

```text
 输入：target = 15
 输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```

**限制：**

* `1 <= target <= 10^5`

&lt;!--more--&gt;

## 2.解题思路

### 2.1 方法一：双指针

> 左右指针，进行计算从左指针到右指针之和，如果小于target，则right指针右移，如果大于target则left右移。如果等于target,则将left到right的数字进行保存。

#### 代码实现

```text
 class Solution57_2 {
     public int[][] findContinuousSequence(int target) {
         List<int[]> vec = new ArrayList<>();
         for (int l = 1, r = 2; l < r;) {
             int sum = (l + r) * (r - l + 1) / 2;
             if (sum == target) {
                 int[] res = new int[r - l + 1];
                 for (int i = l; i <= r; ++i) {
                     res[i - l] = i;
                 }
                 vec.add(res);
                 l++;
             } else if (sum < target) {
                 r++;
             } else {
                 l++;
             }
         }
         return vec.toArray(new int[vec.size()][]);
     }
 }
```

#### 复杂度分析：

> 时间复杂度：O\(target\)
>
> 空间复杂度：O\(1\) \#除答案需要的空间外

## 3.参考

* [https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

