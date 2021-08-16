# 31. 栈的压入、弹出序列

## 1.题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

**示例 1：**

```text
 输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
 输出：true
 解释：我们可以按以下顺序执行：
 push(1), push(2), push(3), push(4), pop() -> 4,
 push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

**示例 2：**

```text
 输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
 输出：false
 解释：1 不能在 2 之前弹出。
```

**提示：**

1. `0 <= pushed.length == popped.length <= 1000`
2. `0 <= pushed[i], popped[i] < 1000`
3. `pushed` 是 `popped` 的排列。

## 2.解题思路

### 2.1 方法一：模拟

> 利用辅助栈进行模拟。

#### 代码实现

```text
 // 使用栈进行模拟
 class Solution31 {
     public boolean validateStackSequences(int[] pushed, int[] popped) {
         Stack<Integer> stack = new Stack<>();
         int index = 0;
         for (int i = 0; i < pushed.length; i++) {
             stack.push(pushed[i]);
             while (!stack.isEmpty() && stack.peek() == popped[index]) {
                 stack.pop();
                 index++;
             }
         }
         return stack.isEmpty();
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

### 2.2 方法二：栈

> 利用pushed数组来进行模拟栈的使用，节省空间。

#### 代码实现

```text
 class Solution31 {
     // 直接把pushed数值作为栈使用。
     public boolean validateStackSequences1(int[] pushed, int[] popped) {
         int top = -1, index = 0;
         for (int i = 0; i < pushed.length; i++) {
             pushed[++top] = pushed[i];
             while (top != -1 && pushed[top] == popped[index]) {
                 top--;
                 index++;
             }
         }
         return top == -1;
     }
 }
```

### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

