# 06. 从尾到头打印链表【递归/辅助栈】

## 1.题目描述

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

**示例 1：**

> 输入：head = \[1,3,2\] 输出：\[2,3,1\]

**限制：**

> 0 &lt;= 链表长度 &lt;= 10000

## 2.解题思路

### 2.1 方法一：递归

> 用递归的方法将链表的值存储到ArrayList数组中，进而将ArrayList数组赋值给res结果数组。

#### 代码实现

```text
 public int[] reversePrint(ListNode head) {
     ArrayList<Integer> tmp = new ArrayList<Integer>();
     recur(head,tmp);
     int[] res = new int[tmp.size()];
     for(int i = 0; i < res.length; i++)
         res[i] = tmp.get(i);
     return res;
 }
 void recur(ListNode head,ArrayList<Integer> tmp) {
     if(head == null) return;
     recur(head.next,tmp);
     tmp.add(head.val);
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

### 2.2 方法二：两次遍历

> 先遍历一遍整个链表，计算出链表的长度。声明链表长度的数组res，再一次遍历链表，将链表值逆序存储到数组中。

#### 代码实现

```text
 public int[] reversePrint2(ListNode head) {
     if(head==null){
         return new int [0];
     }
     int len=0;
     ListNode premmyHead=head;
     while (premmyHead!=null){
         len++;
         premmyHead=premmyHead.next;
     }
     int []res=new int[len];
     int i=1;
     premmyHead=head;
     while(premmyHead!=null){
         res[len-i]=premmyHead.val;
         i++;
         premmyHead=premmyHead.next;
     }
     return res;
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

### 2.3 方法三：辅助栈

> 遍历整个链表将值插入到栈中，遍历完链表后，将栈中元素出栈顺序存储到结果数组res中。

#### 代码实现

```text
 public int[] reversePrint3(ListNode head) {
     Stack<Integer> stack =new Stack<>();
     while(head!=null){
         stack.push(head.val);
         head=head.next;
     }
     int []res=new int[stack.size()];
     for(int i=0;i<res.length;i++){
         res[i]=stack.pop();
     }
     return res;
 }
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

## 参考

* [https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof)
* [https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/solution/mian-shi-ti-06-cong-wei-dao-tou-da-yin-lian-biao-d/](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/solution/mian-shi-ti-06-cong-wei-dao-tou-da-yin-lian-biao-d/) 

