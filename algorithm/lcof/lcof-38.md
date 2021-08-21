# 38. 字符串的排列【回溯】

## 1.题目描述

输入一个字符串，打印出该字符串中字符的所有排列。

 你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

 **示例:**

```text
 输入：s = "abc"
 输出：["abc","acb","bac","bca","cab","cba"]
```

## 2.解题思路

### 2.1 方法一：回溯法

> 回溯思想，由于要求不能出现重复的答案，则对于每一位遍历情况来说，都不能是重复的，所以在遍历每一位的时候，我们使用set来进行存储判断当前位是否已经遍历过。确定当前的字符后，需要将字符和第index位进行交换，回溯后需要将位置换回。

#### 代码实现

```text
/**
 * 回溯
 */
List<String> res = new LinkedList<>();
char[] c;

public String[] permutation(String s) {
    c = s.toCharArray();
    dfs(0);
    return res.toArray(new String[0]);
}
// 交换位置
private void dfs(int index) {
    if (index == c.length - 1) {
        res.add(String.valueOf(c));
        return;
    }
    Set<Character> set = new HashSet<>();
    for (int i = index; i < c.length; i++) {
        if (set.contains(c[i])) {
            continue;
        }
        set.add(c[i]);
        swap(i, index);
        dfs(index + 1);
        swap(i, index);//回溯操作
    }
}

private void swap(int i, int index) {
    char temp = c[i];
    c[i] = c[index];
    c[index] = temp;
}
```

```text
/**
 * 回溯——标记数组
 */
public String[] permutation2(String s) {
    List<String> res = new LinkedList<>();
    int len = s.length();
    boolean[] visited = new boolean[len];
    char[] c = new char[len];
    backtrace(s, 0, visited, c, res, len);
    return res.toArray(new String[0]);
}

private void backtrace(String s, int index, boolean[] visited, char[] c, List<String> res, int len) {
    if (index == len) {
        res.add(String.valueOf(c));
        return;
    }
    Set<Character> set = new HashSet<>();
    for (int i = 0; i < len; i++) {
        if (visited[i] || set.contains(s.charAt(i))) {
            continue;
        }
        visited[i] = true;
        set.add(s.charAt(i));
        c[index] = s.charAt(i);
        backtrace(s, index + 1, visited, c, res, len);
        visited[i] = false;
    }
}
```

#### 复杂度分析

> 时间复杂度：O\(n\*n!\)
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)
* [https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/solution/mian-shi-ti-38-zi-fu-chuan-de-pai-lie-hui-su-fa-by/](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/solution/mian-shi-ti-38-zi-fu-chuan-de-pai-lie-hui-su-fa-by/)

