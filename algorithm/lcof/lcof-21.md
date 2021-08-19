# 21. 调整数组顺序使奇数位于偶数前面【双指针】

## 1.题目描述

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

**示例：**

```text
 输入：nums = [1,2,3,4]
 输出：[1,3,2,4] 
 注：[3,1,2,4] 也是正确的答案之一。
```

**提示：**

1. `0 <= nums.length <= 50000`
2. `1 <= nums[i] <= 10000`

## 2.解题思路

### 2.1 方法一：双指针

> 双指针：left从左往右，right从右往左，分别找到偶数、奇数，交换两数的位置，直到left和right相遇为止，使得奇数都在偶数前面。
>
> **注**：改变了奇数偶数的相对位置顺序。

#### 代码实现

```text
/**
 * 双指针
 */
public int[] exchange(int[] nums) {
    int len = nums.length;
    if (len < 2) {
        return nums;
    }
    int left = 0, right = len - 1;
    while (left < right) {
        while (left < right && (nums[left] & 1) == 1) { // 找到偶数
            left++;
        }
        while (right > left && (nums[right] & 1) == 0) {// 找到奇数
            right--;
        }
        if (left < right) { // 交换位置
            int temp = nums[right];
            nums[right] = nums[left];
            nums[left] = temp;
            left++;
            right--;
        }
    }
    return nums;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* 《剑指offer》
* [https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

