---
description: 剑指offer第24题
---

# 反转链表

 本题同[剑指offer第24题](../lcof/lcof-24.md)一样。



递归做法

```text
 public ListNode reverseList2(ListNode head) {
     if (head == null||head.next==null) {
         return head;
     }
     ListNode newHead = reverseList2(head.next);
     head.next.next=head;//指针转向
     head.next=null;
     return newHead;
 }
```



```text
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode premmyHead=new ListNode(-1);
        premmyHead.next=null;
        while(head!=null){
            ListNode next=head.next;
            head.next=premmyHead.next;
            premmyHead.next=head;
            head=next;
        }
        return premmyHead.next;
    }
}
```

