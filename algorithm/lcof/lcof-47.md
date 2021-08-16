# 47. 礼物的最大价值【DP】

## 1.题目描述

在一个 m\*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

**示例 1:**

```text
 输入: 
 [
   [1,3,1],
   [1,5,1],
   [4,2,1]
 ]
 输出: 12
 解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
```

提示：

* `0 < grid.length <= 200`
* `0 < grid[0].length <= 200`

## 2.解题思路

### 2.1 方法一：动态规划

> 利用动态规划的思想，使得到达每个点的价值都为最大值。

状态转移方程： 选择来自 **上方** 或者 **左边** 最大价值的。$$  
dp\[i\]\[j\]=dp\[i\]\[j\]+Math.max\(dp\[i-1\]\[j\],dp\[i\]\[j-1\]\)  
$$  


#### 代码实现

```text
 class Solution {
     public int maxValue(int[][] grid) {
         if (grid==null||grid.length == 0 || grid[0].length == 0) {
             return -1;
         }
         int rows = grid.length;
         int columns = grid[0].length;
         for (int i = 1; i < columns; i++) {
             grid[0][i] += grid[0][i - 1];
         }
         for (int i = 1; i < rows; i++) {
             grid[i][0] += grid[i - 1][0];
         }
         for (int i = 1; i < rows; i++) {
             for (int j = 1; j < columns; j++) {
                 grid[i][j] += Math.max(grid[i - 1][j], grid[i][j - 1]);
             }
         }
         return grid[rows - 1][columns - 1];
     }
 }
```

#### 复杂度分析

> 时间复杂度：O\(n^2\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

