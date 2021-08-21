# 37. 序列化二叉树【层次遍历】

## 1.题目描述

请实现两个函数，分别用来序列化和反序列化二叉树。

**示例:**

```text
 你可以将以下二叉树：
 ​
     1
    / \
   2   3
      / \
     4   5
 ​
 序列化为 "[1,2,3,null,null,4,5]"
```

## 2.解题思路

### 2.1 方法一：序列化

> 层次遍历—双端队列

#### 代码实现

```text
public String serialize(TreeNode root) {
    if (root == null) {
        return "[]";
    }
    StringBuilder res = new StringBuilder("[");
    Queue<TreeNode> queue = new LinkedList<TreeNode>() {{
        add(root);
    }};
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll();
        if (node != null) {
            res.append(node.val).append(",");
            queue.add(node.left);
            queue.add(node.right);
        } else {
            res.append("null,");
        }
    }
    res.deleteCharAt(res.length() - 1);
    res.append("]");
    return res.toString();
}

public TreeNode deserialize(String data) {
    if (data.equals("[]")) return null;
    String[] values = data.substring(1, data.length() - 1).split(",");
    TreeNode root = new TreeNode(Integer.parseInt(values[0]));
    Queue<TreeNode> queue = new LinkedList<TreeNode>() {{
        add(root);
    }};
    int i = 1;
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll();
        if (!values[i].equals("null")) {
            node.left = new TreeNode(Integer.parseInt(values[i]));
            queue.add(node.left);
        }
        i++;
        if (!values[i].equals("null")) {
            node.right = new TreeNode(Integer.parseInt(values[i]));
            queue.add(node.right);
        }
        i++;
    }
    return root;
}
```

## 3.参考

* [https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

