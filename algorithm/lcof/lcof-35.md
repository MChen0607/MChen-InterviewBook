# 35. 复杂链表的复制\*【哈希表+回溯/迭代】

## 1.题目描述

请实现 `copyRandomList` 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 `next` 指针指向下一个节点，还有一个 `random` 指针指向链表中的任意节点或者 `null`。

**示例 1：**

```text
 输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
 输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**示例 2：**

```text
 输入：head = [[1,1],[2,1]]
 输出：[[1,1],[2,1]]
```

**示例 3：**

```text
 输入：head = [[3,null],[3,0],[3,null]]
 输出：[[3,null],[3,0],[3,null]]
```

**示例 4：**

```text
 输入：head = []
 输出：[]
 解释：给定的链表为空（空指针），因此返回 null。
```

**提示：**

* `-10000 <= Node.val <= 10000`
* `Node.random` 为空（null）或指向链表中的节点。
* 节点数目不超过 1000 。

## 2.解题思路

### 2.1 方法一：哈希表+回溯

> 用哈希表记录每一个节点对应新节点的创建情况。遍历该链表的过程中，检查next和random的创建情况。如果这两个节点中的任何一个节点的新节点没有被创建，我们都立刻递归地进行创建。当我们拷贝完成，回溯到当前层时，我们即可完成当前节点的指针赋值。【dfs思想】。注意一个节点可能被多个其他节点指向，因此可能递归地多次尝试拷贝某个节点，为了防止重复拷贝，需要首先检查当前节点是否被拷贝过，如果已经拷贝过，我们可以直接从哈希表中取出拷贝后的节点的指针并返回即可。
>
> 在实际代码中，我们需要特别判断给定节点为空节点的情况。

#### 代码实现

```text
/**
 * 哈希表+回溯
 */
Map<Node, Node> cachedNode = new HashMap<Node, Node>();

public Node copyRandomList(Node head) {
    if (head == null) {
        return null;
    }
    if (!cachedNode.containsKey(head)) {             // 询问是否已经创建head结点，如果创建则可以直接返回创建的结点
        Node headNew = new Node(head.val);           // 创建新结点
        cachedNode.put(head, headNew);               // 将创建的结点加入到哈希表中
        headNew.next = copyRandomList(head.next);    // 递归创建结点 
        headNew.random = copyRandomList(head.random);// 递归创建结点 
    }
    return cachedNode.get(head);
}
```

#### 复杂度分析

> 时间复杂度：O\(n\) 
>
> 空间复杂度：O\(n\) \# 哈希表

### 2.2 方法二：迭代 + 节点拆分

> 利用迭代的方法进行，先讲所有结点的复制为自己结点的后继结点，那么可以得到每个节点的后继结点就是自己拷贝的结点，对于随机结点可以指向随机结点的后继点，则我们只需要抽离出原先结点的后继结点返回答案即可。

#### 代码实现

```text
/**
 * 迭代 + 节点拆分
 */
public Node copyRandomList2(Node head) {
    if (head == null) {
        return null;
    }
    // 拷贝结点插入到原先结点的后继结点
    for (Node node = head; node != null; node = node.next.next) {
        Node newNode = new Node(node.val);
        newNode.next = node.next;
        node.next = newNode;
    }
    // 复制新结点的random结点，为原先结点的random结点的next结点为新创建的结点。
    for (Node node = head; node != null; node = node.next.next) {
        Node newNode = node.next;
        newNode.random = (node.random != null) ? node.random.next : null;
    }
    // 抽离出新创建的拷贝结点
    Node newHead = head.next;
    for (Node node = head; node != null; node = node.next) {
        Node newNode = node.next;
        node.next = node.next.next; // 还原原先链表
        newNode.next = (newNode.next != null) ? newNode.next.next : null;// 指向拷贝的next结点
    }
    return newHead;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)
* [https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/solution/fu-za-lian-biao-de-fu-zhi-by-leetcode-so-9ik5/](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/solution/fu-za-lian-biao-de-fu-zhi-by-leetcode-so-9ik5/)

