# 41. 数据流中的中位数【最大堆+最小堆】

## 1.题目描述

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

\[2,3,4\] 的中位数是 3

\[2,3\] 的中位数是 \(2 + 3\) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

* void addNum\(int num\) - 从数据流中添加一个整数到数据结构中。
* double findMedian\(\) - 返回目前所有元素的中位数。

**示例 1：**

```text
 输入：
 ["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
 [[],[1],[2],[],[3],[]]
 输出：[null,null,null,1.50000,null,2.00000]
```

**示例 2：**

```text
 输入：
 ["MedianFinder","addNum","findMedian","addNum","findMedian"]
 [[],[2],[],[3],[]]
 输出：[null,null,2.00000,null,2.50000]
```

**限制：**

* 最多会对 `addNum、findMedian` 进行 `50000` 次调用。

## 2.解题思路

### 2.1 方法一：堆 （最大堆+最小堆）

> 用最大堆和最小堆进行，可以将数分为左半堆和右半堆，左半堆数都是小于右半堆堆数。那么，左半堆使用最大堆，右半堆使用最小堆。如果数的总数是偶数，则去两堆的堆顶元素相加除以2。如果总数为奇数，则直接返回左半堆的堆顶元素。
>
> 添加的方法，如果左半堆的数比右半堆数多1，则先将数加入到左半堆最大堆中，去堆顶元素加入到右半堆最小堆中；如果两堆的数相同，则先加入到最小堆（右半堆）中，去堆顶元素移除到最大堆中。

#### 代码实现

```text
/**
 * 用最大堆和最小堆进行。
 */
class MedianFinder {
    PriorityQueue<Integer> maxHeap; // 默认为大根堆
    PriorityQueue<Integer> minHeap;

    /**
     * initialize your data structure here.
     */
    public MedianFinder() {
        maxHeap = new PriorityQueue<>();
        minHeap = new PriorityQueue<>();
    }

    public void addNum(int num) {
        if (maxHeap.size() == minHeap.size()) {
            minHeap.add(num * -1);
            maxHeap.add(minHeap.poll() * -1);
        } else {
            maxHeap.add(num);
            minHeap.add(maxHeap.poll() * -1);
        }
    }

    public double findMedian() {
        if (maxHeap.size() == minHeap.size()) {
            if (maxHeap.size() == 0) {
                return 0;
            }
            return (double) (maxHeap.peek() + minHeap.peek() * -1) / 2;
        } else {
            return maxHeap.peek();
        }
    }
}
```

#### 复杂度分析

> 时间复杂度：计算中位数：O\(1\)  插入：O\(log n\) 
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

