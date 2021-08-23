# 61. 扑克牌中的顺子【排序/哈希表】

## 1.题目描述

从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

**示例1:**

```text
 输入: [1,2,3,4,5]
 输出: True
```

**示例 2:**

```text
 输入: [0,0,1,2,5]
 输出: True
```

**限制：**

* 数组长度为 5
* 数组的数取值为 \[0, 13\] .

## 2.解题思路

### 2.1 方法一：排序

> 统计大小王数量，以及判断是否有非大小王外重复的牌。最后比较最大值和最小值的差是否小于等于4即可。

#### 代码实现

```text
/**
 * 排序
 */
public boolean isStraight(int[] nums) {
    int joker = 0;
    Arrays.sort(nums); // 数组排序
    for (int i = 0; i < 4; i++) {
        if (nums[i] == 0) { // 统计大小王数量
            joker++;
        } else if (nums[i] == nums[i + 1]) { // 若有重复，提前返回 false
            return false;
        }
    }
    return nums[4] - nums[joker] < 5; // 最大牌 - 最小牌 < 5 则可构成顺子
}
```

#### 复杂度分析

> 时间复杂度：O\(n log n\)

> 空间复杂度：O\(1\)

### 2.2 方法二：哈希表

> 使用一个辅助数组来做标记，标记是否已经出现该牌，如果num为0,则跳过，如果出现过则返回false,然后比较得出最大值，最小值。最后比较最大值和最小值的差是否小于等于4即可。

#### 代码实现

```text
/**
 * 哈希表
 */
public boolean isStraight2(int[] nums) {
    boolean[] flag = new boolean[14];
    int min = 13, max = 0;
    for (int num : nums) {
        if (num == 0) {
            continue;
        }
        if (flag[num]) {
            return false;
        }
        max = Math.max(max, num);
        min = Math.min(min, num);
        flag[num] = true;
    }
    return max - min <= 4;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

