# 19. 正则表达式\*【DP/递归】

## 1.题目描述

请实现一个函数用来匹配包含`'.'`和`’*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但与`"aa.a"`和`"ab*a"`均不匹配。

**示例 1:**

> 输入: s = "aa" p = "a" 输出: false 解释: "a" 无法匹配 "aa" 整个字符串。

**示例 2:**

> 输入: s = "aa" p = "a_" 输出: true 解释: 因为 '_' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。

**示例 3:**

> 输入: s = "ab" p = "._" 输出: true 解释: "._" 表示可匹配零个或多个（'\*'）任意字符（'.'）。

**示例 4:**

> 输入: s = "aab" p = "c_a_b" 输出: true 解释: 因为 '\*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。

**示例 5:**

> 输入: s = "mississippi" p = "mis_is_p\*." 输出: false s 可能为空，且只包含从 a-z 的小写字母。 p 可能为空，且只包含从 a-z 的小写字母以及字符 . 和 _，无连续的 '_'。

## 2.解题思路

### 2.1 方法一：动态规划

```text
/**
 * DP
 */
public boolean isMatch2(String A, String B) {
    int n = A.length();
    int m = B.length();
    boolean[][] f = new boolean[n + 1][m + 1];

    for (int i = 0; i <= n; i++) {
        for (int j = 0; j <= m; j++) {
            //分成空正则和非空正则两种
            if (j == 0) {
                f[i][j] = i == 0;
            } else {
                //非空正则分为两种情况 * 和 非*
                if (B.charAt(j - 1) != '*') {
                    if (i > 0 && (A.charAt(i - 1) == B.charAt(j - 1) || B.charAt(j - 1) == '.')) {
                        f[i][j] = f[i - 1][j - 1];
                    }
                } else {
                    //碰到 * 了，分为看和不看两种情况
                    //不看
                    if (j >= 2) {
                        f[i][j] |= f[i][j - 2];
                    }
                    //看
                    if (i >= 1 && j >= 2 && (A.charAt(i - 1) == B.charAt(j - 2) || B.charAt(j - 2) == '.')) {
                        f[i][j] |= f[i - 1][j];
                    }
                }
            }
        }
    }
    return f[n][m];
}
```

#### 复杂度分析

> 时间复杂度：O\(mn\)
>
> 空间复杂度：O\(mn\)

### 2.2 方法二：递归

```text
/**
 * 递归
 */
public boolean isMatch(String A, String B) {
    // 如果字符串长度为0，需要检测下正则串
    if (A.length() == 0) {
        // 如果正则串长度为奇数，必定不匹配，比如 "."、"ab*",必须是 a*b*这种形式，*在奇数位上
        if (B.length() % 2 != 0) {
            return false;
        }
        int i = 1;
        while (i < B.length()) {
            if (B.charAt(i) != '*') {
                return false;
            }
            i += 2;
        }
        return true;
    }
    // 如果字符串长度不为0，但是正则串没了，return false
    if (B.length() == 0) {
        return false;
    }
    // c1 和 c2 分别是两个串的当前位，c3是正则串当前位的后一位，如果存在的话，就更新一下
    char c1 = A.charAt(0), c2 = B.charAt(0), c3 = 'a';
    if (B.length() > 1) {
        c3 = B.charAt(1);
    }
    // 和dp一样，后一位分为是 '*' 和不是 '*' 两种情况
    if (c3 != '*') {
        // 如果该位字符一样，或是正则串该位是 '.',也就是能匹配任意字符，就可以往后走
        if (c1 == c2 || c2 == '.') {
            return isMatch(A.substring(1), B.substring(1));
        } else {
            // 否则不匹配
            return false;
        }
    } else {
        // 如果该位字符一样，或是正则串该位是 '.'，和dp一样，有看和不看两种情况
        if (c1 == c2 || c2 == '.') {
            return isMatch(A.substring(1), B) || isMatch(A, B.substring(2));
        } else {
            // 不一样，那么正则串这两位就废了，直接往后走
            return isMatch(A, B.substring(2));
        }
    }
}

```

#### 复杂度分析

> 时间复杂度：O\(mn\)
>
> 空间复杂度：O\(mn\)

## 3.参考

* [https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof)
* [https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solution/zhu-xing-xiang-xi-jiang-jie-you-qian-ru-shen-by-je/](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solution/zhu-xing-xiang-xi-jiang-jie-you-qian-ru-shen-by-je/)

