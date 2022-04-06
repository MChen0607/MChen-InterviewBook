# 03. 数组中重复的数字【排序/哈希表/数组交换】

## 1.题目描述

找出数组中重复的数字。

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

**示例 1：**

> **输入**：\[2, 3, 1, 0, 2, 5, 3]
>
> **输出**：2 或 3

**限制：**

> 2 <= n <= 100000

## 2.解题思路

### 2.1 方法一:哈希表（Set）

> 使用哈希表来进行，遍历数组，询问哈希表是否存在num元素，如果存在返回重复数字num，否则将num加入到哈希表中
>
> 可以使用标记数组来直接进行，可减少set的查找的时间。

#### 代码实现

```java
 public  int findRepeatNumber(int[] nums) {
     Set<Integer> set = new HashSet<>();
     for (int num : nums) {
         if (set.contains(num)) {
             return num;
         } else {
             set.add(num);
         }
     }
     return -1;
 }
 
/**
 * 标记数组
 */
 public int findRepeatNumber2(int[] nums) {
    boolean[] flag = new boolean[nums.length];
    for (int num : nums) {
        if (flag[num]) {
            return num;
        } else {
            flag[num] = true;
        }
    }
    return -1;
}
```

#### 复杂度分析

> 时间复杂度：O(n) 遍历整个数组
>
> 空间复杂度：O(n) 使用辅助空间Set。

### 2.2 方法二:交换数组位置

> 由于数组是0\~n-1,这个范围恰巧可以与数组的下标一一对应，所以nums\[i]的值为下标i的位置,
>
> 因此本身数组可以作为哈希表来进行使用。
>
> 遍历数组，当数组元素对应则进行下一轮，如果此时下标nums\[i]的值为nums\[i]，则表示有冲突，就意味着找到重复的值，返回即可。如果没有冲突，则将nums\[i]的值交换到对应的位置上。

#### 代码实现：

```java
/**
 * 交换数组位置
 *
 * @param nums 带有重复数字的数组
 * @return 重复的数字
 */
public int findRepeatNumber3(int[] nums) {
    int len = nums.length;
    int i = 0;
    // 一直比较下标i位置的元素，只能下标i对应上了才i++。
    while (i < len) {
        System.out.println(i);
        if (nums[i] == i) {
            i++;
            continue;
        }
        if (nums[nums[i]] == nums[i]) {
            return nums[i];
        }
        swap(nums, i, nums[i]);
        // System.out.println(Arrays.toString(nums));观察数组变换
    }
    return -1;
}

/**
 * a b 为数组下标
 */
private void swap(int[] nums, int a, int b) {
    int temp = nums[a];
    nums[a] = nums[b];
    nums[b] = temp;
}
```

#### 复杂度分析

> 时间复杂度：O(n) 遍历整个数组
>
> 空间复杂度：O(1) 原地算法。

### 2.3 方法三:数组\*

> 通过对遍历的数对应下标的数进行-n，操作，当访问到的数对应下标为负数时，则表示出现重复的数

#### 代码实现

```java
/**
 * 对数对应坐标对数进行-n处理
 */
public int findRepeatNumber5(int[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        int k = nums[i];
        if (k < 0) {
            k += n;
        }
        if (nums[k] < 0) {
            return k;
        }
        nums[k] -= n;
    }
    return -1;
}
```

#### 复杂度分析

> 时间复杂度：O(n) 遍历整个数组
>
> 空间复杂度：O(1) 原地算法。

### 2.4 方法四:排序

> 排序，比较相邻的元素是否相等

代码实现：

```java
/**
 * 排序
 *
 * @param nums 带有重复数字的数组
 * @return 重复的数字
 */
public static int findRepeatNumber4(int[] nums) {
    Arrays.sort(nums);
    int len = nums.length;
    for (int i = 1; i < len; i++) {
        if (nums[i] == nums[i - 1]) {
            return nums[i];
        }
    }
    return -1;
}
```

#### 复杂度分析

> 时间复杂度：O(nlogn) 排序算法O(nlogn)
>
> 空间复杂度：O(1) 原地算法。

**注：** 思考，为什么不能利用方法二中的一一对应的关系来进行比较nums\[i]==i来确定是否在对应的位置;

&#x20;**答：**反例\[1,2,2,3,4,5] 如果是比较一一对应关系，那么返回的结果会是1,由于数组缺失元素，排序后不能保证元素一一对应关系。

## 3.参考

* 《剑指offer》
* [https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof)
* [https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/tong-de-si-xiang-by-liweiwei1419/](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/tong-de-si-xiang-by-liweiwei1419/)
