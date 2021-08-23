# 58 -Ⅰ 翻转单词顺序【字符串】

## 1.题目描述

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

**示例 1：**

```text
 输入: "the sky is blue"
 输出: "blue is sky the"
```

**示例 2：**

```text
 输入: "  hello world!  "
 输出: "world! hello"
 解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```

**示例 3：**

```text
 输入: "a good   example"
 输出: "example good a"
 解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

**说明：**

* 无空格字符构成一个单词。
* 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
* 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

## 2.解题思路

### 2.1 方法一：遍历

> 先去掉字符串首尾的空格，从后往前遍历得到（无空格）字符串后加入到StringBuilder中。

#### 代码实现

```text
/**
 * 遍历
 */
public String reverseWords(String s) {
    s = s.trim(); // 去除头尾空格
    int j = s.length() - 1;
    int i = j;
    StringBuilder stringBuilder = new StringBuilder();
    while (i >= 0) {
        while (i >= 0 && s.charAt(i) != ' ') { // 确定单词的头部
            i--;
        }
        stringBuilder.append(s, i + 1, j + 1).append(' ');
        while (i >= 0 && s.charAt(i) == ' ') { // 确定单词的结尾
            i--;
        }
        j = i;
    }
    return stringBuilder.toString().trim();
}

```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

### 2.2 方法二：split函数分割

> 使用split函数将字符串分割成字符串数组。

#### 代码实现

```text
/**
 * 内置函数
 */
public String reverseWords2(String s) {
    //将传进来的字符串以空格拆分
    String[] strings = s.trim().split(" ");
    StringBuilder stringBuilder = new StringBuilder();
    //从尾巴开始遍历
    for (int i = strings.length - 1; i >= 0; i--) {
        // 两个空格间会被分割成""
        if (strings[i].equals("")) {
            continue;
        }
        if (i == 0) {
            stringBuilder.append(strings[i]);
        } else {
            stringBuilder.append(strings[i]).append(" ");
        }
    }
    return stringBuilder.toString();
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)
* 《剑指offer》

