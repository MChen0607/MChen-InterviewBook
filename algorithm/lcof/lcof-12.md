# 12. 矩阵中的路径【DFS+回溯】

## 1.题目描述

给定一个 `m x n`二维字符网格 board 和一个字符串单词`word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

例如，在下面的 3×4 的矩阵中包含单词 `"ABCCED"`（单词中的字母已标出）。

![img](https://mchen0607.github.io/images/word2.jpg)

**示例 1：**

> **输入**：board = \[\["A","B","C","E"\],\["S","F","C","S"\],\["A","D","E","E"\]\], word = "ABCCED" **输出**：true

**示例 2：**

> **输入**：board = \[\["a","b"\],\["c","d"\]\], word = `"abcd"` **输出**：false

**提示：**

> 1 &lt;= `board.length` &lt;= 200 1 &lt;= `board[i].length` &lt;= 200 board 和 word 仅由大小写英文字母组成

## 2.解题思路

### 2.1 方法一：DFS

> 利用深度优先搜索的方式进行，判断当前位置东南西北方向未访问的点的位置是否和下个字符串相同，如果有一个方向符合要求，那么就朝这个方向继续进行搜索。对不符合要求的方向进行剪枝操作。
>
> 其中运用回溯的思想进行对不符合要求的路径进行结点访问情况恢复。

#### 代码

```text
/**
 * DFS
 */
public boolean exist(char[][] board, String word) {
    if (board.length == 0 || board[0].length == 0) {
        return false;
    }
    char[] arrayWord = word.toCharArray();
    int m = board.length;
    int n = board[0].length; // m行n列
    boolean[][] visited = new boolean[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == arrayWord[0]) {
                visited[i][j] = true;
                if (dfs(board, arrayWord, i, j, m, n, 1, visited)) {
                    return true;
                }
                visited[i][j] = false;
            }
        }
    }
    return false;
}

int[] x = new int[]{0, 1, 0, -1};
int[] y = new int[]{1, 0, -1, 0};

private boolean dfs(char[][] board, char[] arrayWord, int i, int j, int m, int n, int index, boolean[][] visited) {
    if (index == arrayWord.length) {
        return true;
    }
    int indexI = i, indexY = j;
    boolean findPath = false;
    for (int k = 0; k < 4; k++) {
        indexI = i + x[k];
        indexY = j + y[k];
        if (indexI >= 0 && indexI < m && indexY >= 0 && indexY < n && !visited[indexI][indexY] && board[indexI][indexY] == arrayWord[index]) {//剪枝
            visited[indexI][indexY] = true;
            findPath = dfs(board, arrayWord, indexI, indexY, m, n, index + 1, visited);
            if (findPath) {
                return true;
            } else {
                visited[indexI][indexY] = false;
            }
        }
    }
    return false;
}
```

#### 复杂度分析

> 注：复杂度分析参考**官方题解**。

> 时间复杂度：O\(3^{length} MN\) ，length为查找字符串长度。
>
> 空间复杂度：O\(MN\) ，visited数组使用的空间

## 3.参考

* [https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)
* [https://leetcode-cn.com/problems/word-search/solution/dan-ci-sou-suo-by-leetcode-solution/](https://leetcode-cn.com/problems/word-search/solution/dan-ci-sou-suo-by-leetcode-solution/)

