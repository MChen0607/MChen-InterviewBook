# 50. 第一个只出现一次的字符【哈希表/查找】

## 1.题目描述

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

**示例:**

```text
 s = "abaccdeff"
 返回 "b"
 ​
 s = "" 
 返回 " "
```

**限制：**

```text
 0 <= s 的长度 <= 50000
```

## 2.解题思路

### 2.1 方法一：哈希表

> 两边遍历字符串，第一遍统计每个字符出现的次数，第二遍从头开始遍历，判断字符是否出现一次，如果出现次数为1,则返回答案，如果遍历结束后，没有找到答案，则返回' ';

#### 代码实现

```text
/**
 * 哈希表
 */
public char firstUniqChar(String s) {
    int[] nums = new int[95];
    for (int i = 0; i < s.length(); i++) {
        int index = s.charAt(i) - 32;
        nums[index]++;
    }
    for (int i = 0; i < s.length(); i++) {
        int index = s.charAt(i) - 32;
        if (nums[index] == 1) {
            return s.charAt(i);
        }
    }
    return ' ';
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

### 2.2 方法二：查找

> 由于本题字符串只包含小写字母，可以两次查找字符串，分别找出单个字符出现的第一个位置和最后一个位置，时间复杂度为O\(n\).
>
> 如果找到的位置都为同一个位置，则该字符子出现一次，记录该下标；
>
> 如果出现多个只出现一个字符，则保留最小下标的只出现一个字符的下标。
>
> 如果没有出现，则返回' ';

#### 代码实现

```text
/**
 * 查找
 */
public char firstUniqChar2(String s) {
    if (s == null || s.length() == 0)
        return ' ';

    int firstIndex = s.length();
    for (char a = 'a'; a <= 'z'; a++) {
        int i = s.indexOf(a);
        if (i >= 0 && i == s.lastIndexOf(a)) {
            if (firstIndex > i) {
                firstIndex = i;
            }
        }
    }
    return firstIndex == s.length() ? ' ' : s.charAt(firstIndex);
}
// s.indexOf(a);    [从前往后遍历] 找到第一个出现字符就返回下标，找不到就返回-1;
// s.lastIndexOf(a);[从后往前遍历] 找到最后一个出现字符就返回下标，找不到就返回-1;

```

#### 复杂度分析

> 时间复杂度：O\(n\)  \# O\(26n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

