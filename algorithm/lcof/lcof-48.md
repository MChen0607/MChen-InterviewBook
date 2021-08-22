# 48. 最长不重复字符的子字符串【双指针+哈希表】

## 1.题目描述

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

**示例 1:**

```text
 输入: "abcabcbb"
 输出: 3
 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```text
 输入: "bbbbb"
 输出: 1
 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```text
 输入: "pwwkew"
 输出: 3
 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
      请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

提示：

* `s.length <= 40000`

## 2.解题思路

### 2.1 方法一：双指针+哈希表（Map）

 利用双指针加哈希表的方法，哈希表记录最近一次字符的下标，一轮遍历字符串，当发现字符已经出现过，则

1. 计算从left到当前的字符串长度；
2. 设置left指针至（前一个出现的位置 和 left原先指向的位置）的最大值。

#### 代码实现

```text
/**
 * 双指针+哈希表（MAP）
 */
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int len = s.length();
    int ans = 0;
    int left = 0;
    Map<Character, Integer> map = new HashMap<>();
    for (int i = 0; i < len; i++) {
        char c = s.charAt(i);
        if (map.containsKey(c)) {
            ans = Math.max(ans, i - left);
            left = Math.max(map.get(c) + 1, left);
        }
        map.put(c, i);
    }
    ans = Math.max(ans, len - left);
    return ans;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\), 字符的 ASCII 码范围为 0 ~ 127，所以map最多存放128个键值对，O\(128\).

### 2.2 方法二：双指针+哈希表（数组）

> 字符的 ASCII 码范围为 0 ~ 127,所以直接申请128空间的int数组来记录字符的下标

#### 代码实现

```text
/**
 * 双指针+哈希表（数组）
 */
public int lengthOfLongestSubstring2(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int len = s.length();
    int ans = 0;
    int left = 0;
    int[] last = new int[128];
    int f = s.charAt(0);
    for (int i = 1; i < len; i++) {
        int c = s.charAt(i);
        if (last[c] != 0 || c == f) {
            ans = Math.max(ans, i - left);
            left = Math.max(last[c] + 1, left);
        }
        last[c] = i;
    }
    ans = Math.max(ans, len - left);
    return ans;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)
* [https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/solution/mian-shi-ti-48-zui-chang-bu-han-zhong-fu-zi-fu-d-9/](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/solution/mian-shi-ti-48-zui-chang-bu-han-zhong-fu-zi-fu-d-9/)

