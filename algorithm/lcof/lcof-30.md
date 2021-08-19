# 30. 包含min函数的栈

## 1.题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O\(1\)。

**示例:**

```text
 MinStack minStack = new MinStack();
 minStack.push(-2);
 minStack.push(0);
 minStack.push(-3);
 minStack.min();   --> 返回 -3.
 minStack.pop();
 minStack.top();      --> 返回 0.
 minStack.min();   --> 返回 -2.
```

**提示：**

1. 各函数的调用总次数不超过 20000 次

## 2.解题思路

### 2.1 方法一：辅助栈

> 使用两个栈来进行辅助，一个为存储数据的栈，另一个为存储当前数据栈中最小数的栈。

#### 代码实现

```text
/**
 * 两个栈
 */
class MinStack {
    Stack<Integer> stack;
    Stack<Integer> min;

    /**
     * initialize your data structure here.
     */
    public MinStack() {
        stack = new Stack<>();// 数据栈
        min = new Stack<>();  // 最小数栈
    }

    // 添加数据x，如果当前栈为空，则当前添加的数x自然就为最小的数。如果栈不为空，则需要判断当前最小数栈的栈顶元素是否小于x，如果小于x，则继续添加栈顶元素。如果是大于等于x，则需要将x添加到最小数栈中。
    public void push1(int x) {
        if (stack.empty() || x <= min.peek()) {
            stack.push(x);
            min.push(x);
        } else {
            stack.push(x);
            min.push(min.peek());
        }
    }

    // 优化push函数。如果添加数x小于等于最小数栈的栈顶元素，则添加x到最小数栈中，否则不进行操作。节省空间。
    public void push(int x) {
        stack.push(x);
        if (min.empty() || x <= min.peek()) {
            min.push(x);
        }
    }

    // 优化pop函数，如果数据栈的栈顶元素等于最小数栈的栈顶元素，则最小数栈的栈顶元素才需要移除。
    public void pop() {
        if (stack.pop().equals(min.peek())) {
            min.pop();
        }
    }

    // 移除两个栈的栈顶元素
    public void pop1() {
        if (!stack.isEmpty()) {
            stack.pop();
            min.pop();
        }
    }

    // 返回栈顶元素
    public int top() {
        return stack.peek();
    }

    // 如果最小数栈不为空，则返回最小数栈的栈顶元素；否则返回Integer的最小数。
    public int min() {
        if (min.empty()) {
            return Integer.MIN_VALUE;
        } else {
            return min.peek();
        }
    }
}
```

### 2.2 方法二：头插法链表

> 使用链表法，将添加到数据用结点表示，头插法。

#### 方法实现

```text
 class MinStack {
     Node head;
 ​
     /** initialize your data structure here. */
     public MinStack() {
         
     }
     
     public void push(int x) {
         if (head == null) {
             head = new Node(null, x, x);
         } else {
             // 比较当前最小数。
             head = new Node(head, x, Math.min(head.min, x));
         }
     }
     
     // 移除头结点
     public void pop() {
         head = head.next;
     }
     
     public int top() {
         return head.val;
     }
     
     public int min() {
         return head.min;
     }
 }
 ​
 class Node {
     Node next;
     int val;
     int min;
 ​
     public Node(Node next, int val, int min) {
         this.next = next;
         this.val = val;
         this.min = min;
     }
 }
```

## 3.参考

* [https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

