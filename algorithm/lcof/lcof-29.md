# 29. 顺时针打印矩阵

## 1.题目描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

**示例 1：**

```text
 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
 输出：[1,2,3,6,9,8,7,4,5]
```

**示例 2：**

```text
 输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
 输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

**限制：**

* `0 <= matrix.length <= 100`
* `0 <= matrix[i].length <= 100`

## 2.解题思路

### 2.1 方法一：矩阵

> 根据矩阵的大小来进行输出。

#### 代码实现

```text
public int[] spiralOrder(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) {
        return new int[0];
    }
    int rows = matrix.length, columns = matrix[0].length;
    int[] res = new int[rows * columns];
    int index = 0;
    for (int i = 0; 2 * i < columns && 2 * i < rows; i++) { // *条件判断
        index = order(matrix, rows - 1, columns - 1, i, res, index);
    }
    return res;
}

private int order(int[][] matrix, int rows, int columns, int start, int[] res, int index) {
    int endX = rows - start;//行
    int endY = columns - start;//列
    // 遍历矩阵四个方向， 向右，向下，向左，向上。
    // 1.向右：一定存在
    for (int i = start; i <= endY; i++) {
        res[index++] = matrix[start][i];
    }
    // 2.向下：需要有两行以上
    if (endX > start) {
        for (int i = start + 1; i <= endX; i++) {
            res[index++] = matrix[i][endY];
        }
    }
    // 3.向左，需要有两行两列以上
    if (endX > start && endY > start) {
        for (int i = endY - 1; i >= start; i--) {
            res[index++] = matrix[endX][i];
        }
    }
    // 4.向上，需要有三行两列以上
    if (endX > start + 1 && endY > start) {
        for (int i = endX - 1; i > start; i--) {
            res[index++] = matrix[i][start];
        }
    }
    return index;
}
```

#### 复杂度分析

> 时间复杂度：O\(rows\*columns\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

