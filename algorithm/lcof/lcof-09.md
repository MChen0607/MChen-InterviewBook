# 09. 用两个栈实现队列【栈】

## 1.题目描述

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。\(若队列中没有元素，deleteHead 操作返回 -1 \)

**示例 1：**

> 输入： \["CQueue","appendTail","deleteHead","deleteHead"\] \[\[\],\[3\],\[\],\[\]\] 输出：\[null,null,3,-1\]

**示例 2：**

> 输入： \["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"\] \[\[\],\[\],\[5\],\[2\],\[\],\[\]\]
>
> 输出：\[null,-1,null,null,5,2\]

**提示：**

> 1 &lt;= values &lt;= 10000 最多会对 appendTail、deleteHead 进行 10000 次调用

## 2.解题思路

> 模拟，使用两个栈，第一个栈主要入栈，第二个栈用来出栈。
>
> 入栈操作：直接加入到第一个栈中。
>
> 出栈操作：先判断第二个栈中是否有元素，如果有直接出栈就好；如果没有，那么进而判断第一个栈是否有元素：如果有，将第一个栈中的元素依次出栈后入栈到第二栈中，然后再出栈返回结果；如果第一个栈中没有元素，表明两个栈都没有元素，返回-1.

### 2.1 Stack

```text
class CQueue {
    Stack<Integer> stackA;
    Stack<Integer> stackB;

    public CQueue() {
        stackA = new Stack<>();
        stackB = new Stack<>();
    }

    public void appendTail(int value) {
        stackA.push(value);
    }

    public int deleteHead() {
        if (stackB.empty()) {
            while (!stackA.empty()) {
                stackB.push(stackA.pop());
            }
        }
        if (!stackB.empty()) {
            return stackB.pop();
        } else {
            return -1;
        }
    }
}
```

### 2.2 双端队列Deque

> 实现栈的功能

```text
class CQueue2 {
    Deque<Integer> stackA;
    Deque<Integer> stackB;

    public CQueue2() {
        stackA = new LinkedList<>();
        stackB = new LinkedList<>();
    }

    public void appendTail(int value) {
        stackA.addLast(value);
    }

    public int deleteHead() {
        if (stackB.isEmpty()) {
            while (!stackA.isEmpty()) {
                stackB.addLast(stackA.removeLast());
            }
        }
        if (!stackB.isEmpty()) {
            return stackB.removeLast();
        } else {
            return -1;
        }

    }
}
```

#### 复杂度分析

> 时间复杂度：出栈，入栈操作为 O\(1\)
>
> 空间复杂度: O\(n\) 使用了两个辅助栈

## 3.参考

* [https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof)

