# 66. 构建乘积数组【辅助数组】

## 1.题目描述

给定一个数组 `A[0,1,…,n-1]`，请构建一个数组 `B[0,1,…,n-1]`，其中 `B[i]` 的值是数组 `A` 中除了下标 `i` 以外的元素的积, 即 `B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]`。不能使用除法。

**示例:**

```text
 输入: [1,2,3,4,5]
 输出: [120,60,40,30,24]
```

**提示：**

* 所有元素乘积之和不会溢出 32 位整数
* `a.length <= 100000`

## 2.解题思路

### 2.1 方法一：辅助数组

> 由于题目给出不能使用除法操作，那么可以直接先计算出左边的乘积数组和右边的乘积数组。最后将两个数组相乘得出最终结果。

#### 代码实现

```text
/**
 * 辅助数组
 */
public int[] constructArr(int[] a) {
    int len = a.length;
    if (len == 0) {
        return new int[0];
    }
    int[] left = new int[len];
    int[] right = new int[len];
    int[] res = new int[len];
    left[0] = 1;
    right[len - 1] = 1;
    for (int i = 1; i < len; i++) {
        left[i] = left[i - 1] * a[i - 1];
    }
    for (int i = len - 2; i >= 0; i--) {
        right[i] = right[i + 1] * a[i + 1];
    }
    for (int i = 0; i < len; i++) {
        res[i] = left[i] * right[i];
    }
    return res;
}

```

**复杂度分析**

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

### 2.2 优化代码实现

> 优化空间，将left,right数组用一个变量来替代，节省空间开销

```text
/**
 * 优化
 */
public int[] constructArr2(int[] a) {
    int len = a.length;
    if (len == 0) {
        return new int[0];
    }
    int[] res = new int[len];
    int temp = 1;
    // 先将left乘积存入res数组中
    for (int i = 0; i < len; i++) {
        res[i] = temp;
        temp = temp * a[i];
    }
    // 再将right数组乘以res数组。
    temp = 1;
    for (int i = len - 1; i >= 0; i--) {
        res[i] = res[i] * temp;
        temp = temp * a[i];
    }
    return res;
}
```

**复杂度分析**

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\) // 除答案使用空间

## 3.参考

* [https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)

