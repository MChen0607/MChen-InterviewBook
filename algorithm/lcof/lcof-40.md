# 40. 最小的k个数【排序/堆/快排】

## 1.题目描述

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

**示例 1：**

```text
 输入：arr = [3,2,1], k = 2
 输出：[1,2] 或者 [2,1]
```

**示例 2：**

```text
 输入：arr = [0,1,2,1], k = 1
 输出：[0]
```

**限制：**

* `0 <= k <= arr.length <= 10000`
* `0 <= arr[i] <= 10000`

## 2.解题思路

### 2.1 方法一：排序

> 排序，将最小的k个数返回

#### 代码实现

```text
/**
 * 排序
 * 直接复制最小的K个数
 */
public int[] getLeastNumbers(int[] arr, int k) {
    if (k <= 0 || arr.length == 0) {
        return new int[0];
    }
    int[] ans = new int[k];
    Arrays.sort(arr);
    for (int i = 0; i < k; i++) {
        ans[i] = arr[i];
    }
    return ans;
}
```

#### 复杂度分析

> 时间复杂度：O\(n log n \) \#排序
>
> 空间复杂度：O\(1\) \#除去返回结果的空间

### 2.2 方法二：大根堆

> 利用大根堆思想，堆的大小就为k，堆中最大的元素就是堆顶元素，如果比堆顶元素大，那么就是最小的k个数；如果比堆顶元素小，就有可能是最小的k个数，把堆顶元素移除，该数加入到堆中。

#### 代码实现

```text
/**
 * 大根堆
 */
public int[] getLeastNumbers2(int[] arr, int k) {
    if (k <= 0 || arr.length == 0) {
        return new int[0];
    }

    // 大根堆
    PriorityQueue<Integer> queue = new PriorityQueue<>(new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return o2 - o1;
        }
    });
    // 堆的大小为K
    for (int i = 0; i < k; i++) {
        queue.offer(arr[i]);
    }
    for (int i = k; i < arr.length; i++) {
        if (arr[i] < queue.peek()) {
            queue.poll();
            queue.offer(arr[i]);
        }
    }
    int[] ans = new int[k];
    for (int i = 0; i < k; i++) {
        ans[i] = queue.poll();
    }
    return ans;
}
```

#### 复杂度分析

> 时间复杂度：O\(n log k \)
>
> 空间复杂度：O\(k\)

### 2.3 方法三：快排

> 快排+分治

#### 代码实现

```text
/**
 * 快排思想
 */
public int[] getLeastNumbers3(int[] arr, int k) {
    if (k <= 0 || arr.length == 0 || k > arr.length) {
        return new int[0];
    }
    quickSort(arr, 0, arr.length - 1, k - 1);
    int[] ans = new int[k];
    // System.arraycopy(arr, 0, ans, 0, k);
    for (int i = 0; i < k; i++) {
        ans[i] = arr[i];
    }
    return ans;
}
// 将0～k-1划分为最小的k个数
private void quickSort(int[] arr, int left, int right, int k) {
    if (left >= right) {
        return;
    }
    int index = quick(arr, left, right);
//        System.out.println(index);
    if (k == index) {
        return;
    } else if (index < k) {
        quickSort(arr, index + 1, right, k);
    } else {
        quickSort(arr, left, index - 1, k);
    }
}
// 快排实现
private int quick(int[] arr, int left, int right) {
    int key = arr[left];
    int indexLeft = left;
    while (left < right) {
        while (left < right && arr[right] >= key) {
            right--;
        }
        while (left < right && arr[left] <= key) {
            left++;
        }
        if (left < right) {
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
        }
    }
    arr[indexLeft] = arr[left];
    arr[left] = key;
    return left;
}
```

#### 复杂度分析

> 时间复杂度：期望为 O\(n\)，最坏情况下的时间复杂度为 O\(n^2\)【数组已有序】
>
> 空间复杂度：期望为 O\(log n\)，最坏情况下的空间复杂度为 O\(n\)
>
> 注：参考leetcode官方解析

## 3.参考

* [https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)
* [https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/solution/zui-xiao-de-kge-shu-by-leetcode-solution/](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/solution/zui-xiao-de-kge-shu-by-leetcode-solution/)

