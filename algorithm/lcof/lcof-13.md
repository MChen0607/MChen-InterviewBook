# 13. 机器人的运动范围【DFS】

## 1.题目描述

地上有一个m行n列的方格，从坐标 \[0,0\] 到坐标 \[m-1,n-1\] 。一个机器人从坐标 \[0, 0\] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 \[35, 37\] ，因为3+5+3+7=18。但它不能进入方格 \[35, 38\]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

**示例 1：**

> 输入：m = 2, n = 3, k = 1 输出：3

**示例 2：**

> 输入：m = 3, n = 1, k = 0 输出：1

**提示：**

> 1 &lt;= n,m &lt;= 100 0 &lt;= k &lt;= 20

## 2.解题思路

### 2.1 方法一：DFS

> 本题思路和[剑指offer 12.矩阵的路径](lcof-12.md)是一样的，采用深度优先搜索策略。
>
> 只是在剪枝判断过程时，增加结点的数位之后是否小于K\(18\)。其他思路大致相同。
>
> 本题机器人走两个方向，一个是向右，一个向下即可。

### 代码实现

```text
/**
 * DFS
 */
public int movingCount(int m, int n, int k) {
    if (m == 0 || n == 0) {
        return 0;
    }
    boolean[][] visited = new boolean[m][n];
    return dfs(m, n, k, 0, 0, visited);
}

private int dfs(int m, int n, int k, int indexI, int indexJ, boolean[][] visited) {
    int count = 0;
    if (indexI >= 0 && indexI < m && indexJ >= 0 && indexJ < n
            && !visited[indexI][indexJ] && getSum(indexI) + getSum(indexJ) <= k) { //indexI、indexJ一定是 >=0的,剪枝。
        visited[indexI][indexJ] = true;
        count = 1 + dfs(m, n, k, indexI + 1, indexJ, visited) + dfs(m, n, k, indexI, indexJ + 1, visited);
    }
    return count;
}

private int getSum(int n) {
    int sum = 0;
    while (n != 0) {
        sum += n % 10;
        n = n / 10;
    }
    return sum;
}
```

### 复杂度分析

> 时间复杂度：O\(MN\)
>
> 空间复杂度：O\(MN\)

## 3.参考

* [https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof)

