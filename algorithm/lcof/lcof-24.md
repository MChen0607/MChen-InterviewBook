# 24. 反转链表【递归/迭代】

## 1.题目描述

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

**示例:**

```text
 输入: 1->2->3->4->5->NULL
 输出: 5->4->3->2->1->NULL
```

**限制：**

```text
 0 <= 节点个数 <= 5000
```

## 2.解题思路

### 2.1 方法一：迭代

> 迭代法：定义一个伪头部，遍历head链表，使用头插法插入到伪头部链表中，最后返回虚拟头部的next指针即可。

#### 代码实现

```text
 public ListNode reverseList(ListNode head) {
     ListNode dummyHead = new ListNode(-1);
     dummyHead.next = null;
     while (head != null) {
         ListNode next = head.next;
         head.next = dummyHead.next;
         dummyHead.next = head;
         head = next;
     }
     return dummyHead.next;
 }
```

> 不需要创建伪结点，直接通过双指针的方法来进行。

```text
public ListNode reverseList3(ListNode head) {
        ListNode cur = head, pre = null;
        while (cur != null) {
            ListNode tmp = cur.next; // 暂存后继节点 cur.next
            cur.next = pre;          // 修改 next 引用指向
            pre = cur;               // pre 暂存 cur
            cur = tmp;               // cur 访问下一节点
        }
        return pre;
    }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

### 2.2 方法二\*：递归

> 递归调用，修改结点的指针转向。

#### 代码实现

```text
 public ListNode reverseL ist2(ListNode head) {
     if (head == null||head.next==null) {
         return head;
     }
     ListNode newHead = reverseList2(head.next);
     head.next.next=head;//指针转向
     head.next=null;
     return newHead;
 }
```

> 定义函数形式递归

```text
public ListNode reverseList4(ListNode head) {
    return recur(head, null);
}

private ListNode recur(ListNode cur, ListNode pre) {
    if (cur == null) {
        return pre;
    }
    ListNode res = recur(cur.next, cur);
    cur.next = pre;
    return res;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\) //递归调用栈使用。

## 3.参考

* [https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)
* [https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/solution/fan-zhuan-lian-biao-by-leetcode-solution-jvs5/](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/solution/fan-zhuan-lian-biao-by-leetcode-solution-jvs5/)
* [https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/solution/jian-zhi-offer-24-fan-zhuan-lian-biao-die-dai-di-2/](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/solution/jian-zhi-offer-24-fan-zhuan-lian-biao-die-dai-di-2/)

