# 11. 旋转数组的最小数字【二分查找】

## 1.题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 \[3,4,5,1,2\] 为 \[1,2,3,4,5\] 的一个旋转，该数组的最小值为1。

**示例 1：**

> 输入：\[3,4,5,1,2\] 输出：1

**示例 2：**

> 输入：\[2,2,2,0,1\] 输出：0

## 2.解题思路

### 2.1 方法一：暴力

> 由于旋转数组的呈现状只有两种，一种为旋转后保存原样的数组形式，另一种为 数组可分为两段递增排序，其中中断点定义为最大值到最小值的中断。
>
> 所以直接遍历数组，查看数组中是否有出现中断现象（当前值小于上一个值），如果有，则当前值为最小值，如果没有出现该现象，则第一个元素为最小值。

#### 代码实现

```text
/**
 * 查找是否有出现断层现象
 *
 * @param numbers 旋转数组
 * @return 最小的数字
 */
public int minArray(int[] numbers) {
    if (numbers == null) {
        return -1;
    }
    for (int i = 1; i < numbers.length; i++) {
        if (numbers[i] < numbers[i - 1]) {
            return numbers[i];
        }
    }
    return numbers[0];
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

### 2.2 方法二：二分查找

> 二分查找，对方法一的优化，方法一遍历数组中间复杂度为O\(n\),使用二分查找的方法来查找中断点时间复杂度只需要O\(logn\)。
>
> 二分查找规则：以right元素为比较对象，根据旋转数组的规律进行比较。
>
> * mid元素&gt;right元素，则最小值一定存在于mid右部分。
> * mid元素&lt;right元素,则最小值一定存在于mid的左部分。
> * mid元素==right元素,无法判断存在于哪个部分，则移动right向左移动一位，不影响最后结果，如果right为最小值，mid也是最小值，所以移动right一位不影响结果。

#### 代码实现

```text
/**
 * 二分查找
 */
public int minArray2(int[] numbers) {
    int left = 0, right = numbers.length - 1;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (numbers[mid] < numbers[right]) {
            right = mid;
        } else if (numbers[mid] > numbers[right]) {
            left = mid + 1;
        } else {
            right -= 1;
        }
    }
    return numbers[left];
}


/**
 * 二分查找优化
 * 当nums[mid]=nums[right],可能会出现此时left～mid~right全都相等，所以可以直接线性查找
 */
public int minArray3(int[] numbers) {
    int left = 0, right = numbers.length - 1;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (numbers[mid] < numbers[right]) {
            right = mid;
        } else if (numbers[mid] > numbers[right]) {
            left = mid + 1;
        } else {
            int index = left;
            for (int i = left + 1; i < right; i++) {
                if (numbers[i] < numbers[index]) {
                    index = i;
                }
            }
            return numbers[index];
        }
    }
    return numbers[left];
}
```

#### 复杂度分析

> 时间复杂度：O\(log n\) // 当所有数都相等时，会退化为O\(n\)。
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof)

