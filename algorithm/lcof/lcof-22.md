# 22. 链表中倒数第k个节点【快慢指针】

## 1.题目描述

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 `6` 个节点，从头节点开始，它们的值依次是 `1、2、3、4、5、6`。这个链表的倒数第 `3` 个节点是值为 `4` 的节点。

**示例：**

```text
 给定一个链表: 1->2->3->4->5, 和 k = 2.
 ​
 返回链表 4->5.
```

## 2.解题思路

### 2.1 方法一：双指针\[快慢指针\]

> 双指针，使用快慢指针进行，快指针post先走K步后，慢指针cur同快指针post一同向前走，直到快指针走到尽头，此时慢指针就是倒数第K个指针。

#### 代码实现

```text
 // 假设此方法 k 是有效的，解决本题足以。
 public ListNode getKthFromEnd(ListNode head, int k) {
     ListNode post = head, cur = head;
     for (int i = 0; i < k; i++) {
         post = post.next;
     }
     while (post != null) {
         cur = cur.next;
         post = post.next;
     }
     return cur;
 }
```

```text
 // 假设此方法 k 是任意数
 // 先计算链表长度，判断k是否有效
 public ListNode getKthFromEnd2(ListNode head, int k) {
     if (k < 1) { //本题从1开始
         return null;
     }
     int len = 0;
     ListNode dummyHead = head;
     while (dummyHead != null) {//计算链表的长度
         len++;
         dummyHead = dummyHead.next;
     }
     if (k < 1 || k > len) { //k超出1~len的范围为无效范围
         return null;
     }
     ListNode post = head, cur = head;
     for (int i = 0; i < k; i++) {
         post = post.next;
     }
     while (post != null) {
         cur = cur.next;
         post = post.next;
     }
     return cur;
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)
* 《剑指offer》

