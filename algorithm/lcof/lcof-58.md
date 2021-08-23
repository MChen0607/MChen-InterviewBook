# 58 -Ⅱ 左旋转字符串【字符串】

## 1.题目描述

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

**示例 1：**

```text
 输入: s = "abcdefg", k = 2
 输出: "cdefgab"
```

**示例 2：**

```text
 输入: s = "lrloseumgh", k = 6
 输出: "umghlrlose"
```

**限制：**

* `1 <= k < s.length <= 10000`

## 2.解题思路

### 2.1 方法一:字符串

> len长度字符串，先将n~len加入到StringBuilder，后将0到n加入到StringBuilder中。

#### 代码实现

```text
/**
 * StringBuilder
 */
public String reverseLeftWords(String s, int n) {
    StringBuilder stringBuilder = new StringBuilder();
    int len = s.length();
    n = n % len;
    if (n == 0) {
        return s;
    }
    for (int i = n; i < len; i++) {
        stringBuilder.append(s.charAt(i));
    }
    for (int i = 0; i < n; i++) {
        stringBuilder.append(s.charAt(i));
    }
    // 取余操作
    //for (int i = n; i < n+len; i++) {
    //   stringBuilder.append(s.charAt(i%len));
    //}
    return stringBuilder.toString();
}
```

#### 复杂度分析

> 时间复杂度:O\(n\)
>
> 空间复杂度:O\(n\)

### 2.2 方法二:substring\(\)

> 使用String内置substring函数

#### 代码实现

```text
/**
 * substring
 */
public String reverseLeftWords2(String s, int n) {
    int a = s.length();
    String str = s.substring(0, n);
    String str2 = s.substring(n, a);
    return str2 + str;
}
```

#### 复杂度分析

> 时间复杂度:O\(n\)
>
> 空间复杂度:O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

