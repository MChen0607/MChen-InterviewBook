# 59 -Ⅱ 队列的最大值【设计+队列】

## 1.题目描述

请定义一个队列并实现函数 `max_value` 得到队列里的最大值，要求函数`max_value`、`push_back` 和 `pop_front` 的**均摊**时间复杂度都是O\(1\)。

若队列为空，`pop_front` 和 `max_value` 需要返回 -1

**示例 1：**

```text
 输入: 
 ["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
 [[],[1],[2],[],[],[]]
 输出: [null,null,null,2,1,2]
```

**示例 2：**

```text
 输入: 
 ["MaxQueue","pop_front","max_value"]
 [[],[],[]]
 输出: [null,-1,-1]
```

**限制：**

* `1 <= push_back,pop_front,max_value的总操作数 <= 10000`
* `1 <= value <= 10^5`

## 2.解题思路

### 2.1 方法一：队列

> 使用双端队列来保存最大值，如果加入的值比双端队列的尾部大，则将尾部元素退出，尾部元素**大于等于**即将加入的值或者双端队列为**空**。

#### 代码实现

```text
 class MaxQueue {
     Queue<Integer> q;
     Deque<Integer> d;
 ​
     public MaxQueue() {
         q = new LinkedList<Integer>();
         d = new LinkedList<Integer>();
     }
     
     public int max_value() {
         if (d.isEmpty()) {
             return -1;
         }
         return d.peekFirst();
     }
     
     public void push_back(int value) {
         // 将加入的值加入到双端队列中
         while (!d.isEmpty() && d.peekLast() < value) {
             d.pollLast();
         }
         
         d.offerLast(value);
         q.offer(value);
     }
     
     public int pop_front() {
         if (q.isEmpty()) {
             return -1;
         }
         int ans = q.poll();
         if (ans == d.peekFirst()) {
             d.pollFirst();
         }
         return ans;
     }
 }
```

#### 复杂度分析

> `max_value`、`push_back` 和 `pop_front` 的**均摊**时间复杂度都是O\(1\)。
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

