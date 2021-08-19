# 25. 合并两个排序的链表【迭代/递归】

## 1.题目描述

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

**示例1：**

```text
 输入：1->2->4, 1->3->4
 输出：1->1->2->3->4->4
```

**限制：**

```text
 0 <= 链表长度 <= 1000
```

## 2.解题思路

### 2.1 方法一：迭代

> 使用伪头结点，比较两个链表，将值小的链条通过尾插入法 插入到伪结点的链表中，直到其中一方结束，接着将不为空的一方直接加入到伪结点的链表的尾部。

#### 代码实现

```text
/**
 * 迭代
 */
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(-1);
    ListNode cur = dummyHead;
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) {
            cur.next = l1;
            l1 = l1.next;
        } else {
            cur.next = l2;
            l2 = l2.next;
        }
        cur = cur.next;
    }
    cur.next = l1 == null ? l2 : l1;
    return dummyHead.next;
}
```

#### 复杂度分析

> 时间复杂度：O\(m+n\)
>
> 空间复杂度：O\(1\)

### 2.2 方法二：递归

#### 代码实现

```text
/**
 * 递归
 */
public ListNode mergeTwoLists2(ListNode l1, ListNode l2) {
    if (l1 == null) {
        return l2;
    }
    if (l2 == null) {
        return l1;
    }
    ListNode res = null;
    if (l1.val <= l2.val) {
        res = l1;
        res.next = mergeTwoLists2(l1.next, l2);
    } else {
        res = l2;
        res.next = mergeTwoLists2(l1, l2.next);
    }
    return res;
}
```

#### 复杂度分析

> 时间复杂度：O\(m+n\)
>
> 空间复杂度：O\(n+m\) //递归调用栈

## 3.参考

* [https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)
* 《剑指offer》

