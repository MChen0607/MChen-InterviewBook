# 18. 删除链表的节点【快慢指针】

## 1.题目描述

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

**示例 1:**

> 输入: head = \[4,5,1,9\], val = 5 输出: \[4,1,9\] 解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -&gt; 1 -&gt; 9.

**示例 2:**

> 输入: head = \[4,5,1,9\], val = 1 输出: \[4,5,9\] 解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -&gt; 5 -&gt; 9.

**说明：**

* 题目保证链表中节点的值互不相同

## 2.解题思路

### 2.1 方法一：双指针\[快慢指针\]

> 使用双指针的方式，cur指向当前结点，pre指向当前结点的前一个结点，一次遍历链表即可。

#### 代码

```text
 class Solution {
     public ListNode deleteNode(ListNode head, int val) {
         // 如果链表为空，则直接返回null结点。
         if(head==null){
             return null;
         }
         // 如果要删除的结点就是头结点，可直接返回头结点的next指针。
         if(head.val==val){
             return head.next;
         }
         // 如果不为以上两种情况，则使用双指针的方式进行对链表的删除结点任务
         // pre记为要删除结点的前一个指针，cur为当前遍历的指针。
         // 循环遍历直到cur为null.
         ListNode pre=head,cur=head.next;
         while(cur!=null){
             if(cur.val==val){// 如果找到要删除的结点，则将当前的next指针赋值给pre的next指针，完成删除工作。
                 pre.next=cur.next;
                 return head;
             }
             pre=cur;
             cur=cur.next;
         }
         return head;//如果没找要删除的结点，则返回head结点即可。
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\) //链表一轮遍历
>
> 空间复杂度：O\(1\) //只使用了 pre 、cur指针

## 3.参考

* [https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof)

