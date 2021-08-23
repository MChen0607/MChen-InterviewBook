# 52. 两个链表的第一个公共节点【双指针】

## 1.题目描述

输入两个链表，找出它们的第一个公共节点。

如下面的两个链表**：**

![](../../.gitbook/assets/160_statement.png)

在节点 c1 开始相交。

**示例 1：**

![](../../.gitbook/assets/160_example_1.png)

```text
 输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
 输出：Reference of the node with value = 8
 输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。
 从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
 在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

**示例 2：**

![](../../.gitbook/assets/160_example_3.png)

```text
 输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
 输出：null
 输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
 由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
 解释：这两个链表不相交，因此返回 null。
```

**注意**

* 如果两个链表没有交点，返回 `null`.
* 在返回结果后，两个链表仍须保持原有的结构。
* 可假定整个链表结构中没有循环。
* 程序尽量满足 O\(_n_\) 时间复杂度，且仅用 O\(_1_\) 内存。

## 2.解题思路

### 2.1 方法一：模拟+双指针

> 先计算出两个链表的长度，让长的一方先走 两方的距离差，进而像快慢指针一样一起往前走，判断是否相等

#### 代码实现

```text
/**
 * 双指针
 */
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int sizeA = 0, sizeB = 0;
    // 计算A链表的长度
    ListNode nodeA = headA;
    while (nodeA != null) {
        sizeA++;
        nodeA = nodeA.next;
    }
    // 计算B链表的长度
    ListNode nodeB = headB;
    while (nodeB != null) {
        sizeB++;
        nodeB = nodeB.next;
    }
    if (sizeA == 0 || sizeB == 0) {
        return null;
    }
    // 长度长的链表先前进长度差步数
    nodeA = headA;
    nodeB = headB;
    if (sizeA > sizeB) {
        int len = sizeA - sizeB;
        while (len > 0) {
            len -= 1;
            nodeA = nodeA.next;
        }
    } else if (sizeB > sizeA) {
        int len = sizeB - sizeA;
        while (len > 0) {
            len -= 1;
            nodeB = nodeB.next;
        }
    }
    // 链表A、B共同前进，判断是否相等
    while (nodeA != null && nodeB != null) {
        if (nodeA == nodeB) {
            return nodeA;
        }
        nodeA = nodeA.next;
        nodeB = nodeB.next;
    }
    return null;
}
```

#### 复杂度分析

> 时间复杂度：O\(a+b\)
>
> 空间复杂度：O\(1\)

### 2.2 方法二：模拟+双指针

> 两方在各自的链表上顺序遍历一步，遍历完自己后遍历对方的，直到找到共同节点或者为null。当走到对方上，走的步数是相同的（a+b\)-c;c为共同结点的个数。

#### 代码实现

```text
/**
 * 双指针
 */
public ListNode getIntersectionNode2(ListNode headA, ListNode headB) {
    ListNode A = headA, B = headB;
    while (A != B) {
        A = A != null ? A.next : headB;
        B = B != null ? B.next : headA;
    }
    return A;
}
```

#### 复杂度分析

> 时间复杂度：O\(a+b\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)
* [https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/solution/jian-zhi-offer-52-liang-ge-lian-biao-de-gcruu/](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/solution/jian-zhi-offer-52-liang-ge-lian-biao-de-gcruu/)

