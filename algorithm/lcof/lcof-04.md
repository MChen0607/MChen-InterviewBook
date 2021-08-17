# 04. 二维数组中的查找【线性查找】

## 1.题目描述

在一个 n \* m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

**示例:**

现有矩阵 matrix 如下：

> \[ \[1, 4, 7, 11, 15\], \[2, 5, 8, 12, 19\], \[3, 6, 9, 16, 22\], \[10, 13, 14, 17, 24\], \[18, 21, 23, 26, 30\] \]

给定 target = 5，返回 true。

给定 target = 20，返回 false。

**限制：**

0 &lt;= n &lt;= 1000

0 &lt;= m &lt;= 1000

## 2.解题思路

### 2.1 方法一：暴力法

> 暴力解：直接双重循环遍历二维数组。

#### 代码实现

```text
 public boolean findNumberIn2DArray(int[][] matrix, int target) {
     if (matrix.length == 0 || matrix[0].length == 0) {
         return false;
     }
     int m = matrix.length;
     int n = matrix[0].length;
     for (int i = 0; i < m; i++) {
         for (int j = 0; j < n; j++) {
             if (matrix[i][j] == target)
                 return true;
         }
     }
     return false;
 }
```

#### 复杂度分析

> 时间复杂度 O\(mn\) 双重循环
>
> 空间复杂度 O\(1\)

### 2.2 方法二：线性查找【二叉排序树】

> 矩阵中神奇的发现，选择矩阵后可得到二叉排序树。
>
> 线性查找，**将矩阵旋转可得到 二叉排序树**，左孩子结点小于根节点，右节点大于根节点。
>
> 利用这个规律：
>
> 初始化起点：矩阵左上角点\(0,n-1\)。
>
> 如果结点值 等于 target : return true
>
> 如果结点值 大于 target ：列数减一
>
> 如果结点值 小于 target : 行数加一

#### 代码实现

```text
 public boolean findNumberIn2DArray2(int[][] matrix, int target) {
     if(matrix==null||matrix.length==0||matrix[0].length==0){
         return false;
     }
     int m=matrix.length;
     int n=matrix[0].length;
     int i=0,j=n-1;
     while(i<m&&j>=0){
         if(matrix[i][j]==target){
             return true;
         }else if(matrix[i][j]>target){
             j--;
         }else{
             i++;
         }
     }
     return false;
 }
```

#### 复杂度分析

> 时间复杂度O\(m+n\) 最差情况，从矩阵右上角到左下角。
>
> 空间复杂度O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)
* [https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/solution/mian-shi-ti-04-er-wei-shu-zu-zhong-de-cha-zhao-b-3/](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/solution/mian-shi-ti-04-er-wei-shu-zu-zhong-de-cha-zhao-b-3/)
* [https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/solution/mian-shi-ti-04-er-wei-shu-zu-zhong-de-cha-zhao-zuo/](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/solution/mian-shi-ti-04-er-wei-shu-zu-zhong-de-cha-zhao-zuo/) 

