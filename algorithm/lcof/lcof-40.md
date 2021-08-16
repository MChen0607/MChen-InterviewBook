# 40. 最小的k个数

## 1.题目描述

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

**示例 1：**

```text
 输入：arr = [3,2,1], k = 2
 输出：[1,2] 或者 [2,1]
```

**示例 2：**

```text
 输入：arr = [0,1,2,1], k = 1
 输出：[0]
```

**限制：**

* `0 <= k <= arr.length <= 10000`
* `0 <= arr[i] <= 10000`

## 2.解题思路

### 2.1 方法一：排序

### 2.2 方法二：最小堆

### 2.3 方法三：快排

#### 代码实现

```text
 class Solution40 {
     // 排序 直接复制最小的K个数
     public int[] getLeastNumbers(int[] arr, int k) {
         if (k <= 0 || arr.length == 0) {
             return new int[0];
         }
         int[] ans = new int[k];
         Arrays.sort(arr);
         // System.arraycopy(arr, 0, ans, 0, k);
         for (int i = 0; i < k; i++) {
             ans[i] = arr[i];
         }
         return ans;
     }
 ​
     // 堆排序
     public int[] getLeastNumbers2(int[] arr, int k) {
         if (k <= 0 || arr.length == 0) {
             return new int[0];
         }
 ​
         // 最小堆 TODO 确认compare的作用
         PriorityQueue<Integer> queue = new PriorityQueue<>(new Comparator<Integer>() {
             @Override
             public int compare(Integer o1, Integer o2) {
                 return o2 - o1;
             }
         });
         // 堆的大小为K
         for (int i = 0; i < k; i++) {
             queue.offer(arr[i]);
         }
         for (int i = k; i < arr.length; i++) {
             if (arr[i] < queue.peek()) {
                 queue.poll();
                 queue.offer(arr[i]);
             }
         }
         int[] ans = new int[k];
         for (int i = 0; i < k; i++) {
             ans[i] = queue.poll();
         }
         return ans;
     }
 ​
     // 快排思想
     public int[] getLeastNumbers3(int[] arr, int k) {
         if (k <= 0 || arr.length == 0 || k > arr.length) {
             return new int[0];
         }
         quickSort(arr, 0, arr.length - 1, k - 1);
         int[] ans = new int[k];
         // System.arraycopy(arr, 0, ans, 0, k);
         for (int i = 0; i < k; i++) {
             ans[i] = arr[i];
         }
         return ans;
     }
 ​
 ​
     private void quickSort(int[] arr, int left, int right, int k) {
         if (left >= right) {
             return;
         }
         int index = quick(arr, left, right);
         System.out.println(index);
         if (k == index) {
             return;
         } else if (index < k) {
             quickSort(arr, index + 1, right, k);
         } else {
             quickSort(arr, left, index - 1, k);
         }
     }
 ​
     private int quick(int[] arr, int left, int right) {
         int key = arr[left];
         int indexLeft = left;
         while (left < right) {
             while (left < right && arr[right] >= key) {
                 right--;
             }
             while (left < right && arr[left] <= key) {
                 left++;
             }
             if (left < right) {
                 int temp = arr[left];
                 arr[left] = arr[right];
                 arr[right] = temp;
             }
         }
         arr[indexLeft] = arr[left];
         arr[left] = key;
         return left;
     }
 }
```

## 3.参考

* [https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

