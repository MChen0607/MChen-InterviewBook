# 59-Ⅰ 滑动窗口的最大值\*【最大堆/单调队列/分块】

## 1.题目描述

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

示例:

输入: nums = \[1,3,-1,-3,5,3,6,7\], 和 k = 3 输出: \[3,3,5,5,6,7\] 解释:

```text
  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

提示：

你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

## 2.解题思路

### 2.1 方法一：优先级队列--最大堆

> 使用最大堆来进行维持滑动窗口大小的堆，在确保堆顶元素在滑动窗口内的话，堆顶元素就是滑动窗口的最大值。

#### 代码实现

```text
/**
 * 最大堆
 */
public int[] maxSlidingWindow(int[] nums, int k) {
    int n = nums.length;
    PriorityQueue<int[]> pq = new PriorityQueue<int[]>(new Comparator<int[]>() {
        public int compare(int[] pair1, int[] pair2) {
            return pair1[0] != pair2[0] ? pair2[0] - pair1[0] : pair2[1] - pair1[1];
        }
    });
    for (int i = 0; i < k; ++i) {
        pq.offer(new int[]{nums[i], i});
    }
    int[] ans = new int[n - k + 1];
    ans[0] = pq.peek()[0];
    for (int i = k; i < n; ++i) {
        pq.offer(new int[]{nums[i], i}); // 添加元素
        while (pq.peek()[1] <= i - k) {  // 移除元素，使得堆顶元素在滑动窗口内
            pq.poll();
        }
        ans[i - k + 1] = pq.peek()[0];
    }
    return ans;
}
```

#### 复杂度分析

> 时间复杂度:O\(n log n\)
>
> 空间复杂度:O\(n\)

### 2.2 方法二：单调队列

> 单调队列中存储递减数的下标。当加入的元素比队列尾部元素大时，则移除尾部元素，直到队列为空，或者存在比访问元素大。将队列中头部不在滑动窗口的元素移除。

#### 代码实现

```text
/**
 * 单调队列
 */
public int[] maxSlidingWindow2(int[] nums, int k) {
    int n = nums.length;
    Deque<Integer> deque = new LinkedList<Integer>();
    for (int i = 0; i < k; ++i) {
        while (!deque.isEmpty() && nums[i] >= nums[deque.peekLast()]) {
            deque.pollLast();
        }
        deque.offerLast(i);
    }
    System.out.println(Arrays.toString(deque.toArray()));
    int[] ans = new int[n - k + 1];
    ans[0] = nums[deque.peekFirst()];
    for (int i = k; i < n; ++i) {
        while (!deque.isEmpty() && nums[i] >= nums[deque.peekLast()]) { // 确保队列为单调递减队列
            deque.pollLast();
        }
        deque.offerLast(i);
        while (deque.peekFirst() <= i - k) { // 确保队列头部元素在滑动窗口范围内
            deque.pollFirst();
        }
        ans[i - k + 1] = nums[deque.peekFirst()];
    }
    return ans;
}
```

#### 复杂度分析

> 时间复杂度:O\(n\)
>
> 空间复杂度:O\(k\)

### 2.3 方法三：分块

#### 代码实现

```text
/**
 * 分块
 */
public int[] maxSlidingWindow3(int[] nums, int k) {
    int n = nums.length;
    int[] prefixMax = new int[n];
    int[] suffixMax = new int[n];
    for (int i = 0; i < n; ++i) {
        if (i % k == 0) {
            prefixMax[i] = nums[i];
        } else {
            prefixMax[i] = Math.max(prefixMax[i - 1], nums[i]);
        }
    }
    for (int i = n - 1; i >= 0; --i) {
        if (i == n - 1 || (i + 1) % k == 0) {
            suffixMax[i] = nums[i];
        } else {
            suffixMax[i] = Math.max(suffixMax[i + 1], nums[i]);
        }
    }

    int[] ans = new int[n - k + 1];
    for (int i = 0; i <= n - k; ++i) {
        ans[i] = Math.max(suffixMax[i], prefixMax[i + k - 1]);
    }
    return ans;
}
```

#### 复杂度分析

> 时间复杂度:O\(n\)
>
> 空间复杂度:O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)
* [https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/solution/hua-dong-chuang-kou-de-zui-da-zhi-by-lee-ymyo/](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/solution/hua-dong-chuang-kou-de-zui-da-zhi-by-lee-ymyo/)

