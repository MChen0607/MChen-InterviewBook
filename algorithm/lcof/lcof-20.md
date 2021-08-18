# 20. 表示数值的字符串【字符串】

## 1.题目描述

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"-1E-16"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"及"12e+5.4"都不是。

## 2.解题思路

### 2.1 方法一：字符串模拟

> 根据字符设置不同的状态，进行状态间的转换和判断。

#### 代码

```text
public boolean isNumber(String s) {
    if (s.length() == 0) {
        return false;
    }
    s = s.trim();//先去除字符串前后的空格
    boolean numFlag = false;
    boolean opsFlag = false;
    boolean eFlag = false;
    boolean dotFlag = false;
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (c >= '0' && c <= '9') {
            numFlag = true;
        } else if (c == '.' && !eFlag && !dotFlag) {//e后面不能有'.'
            dotFlag = true;
        } else if ((c == '+' || c == '-') && !opsFlag && (i == 0 || s.charAt(i - 1) == 'e' || s.charAt(i - 1) == 'E')) {
            //只能有一个'+'/'-';获取前一个字符为'e'/'E';
            opsFlag = true;
        } else if ((c == 'e' || c == 'E') && !eFlag && numFlag) {//e前面至少要有一个数组出现。
            eFlag = true;// 合格的数字只能有一个e
            opsFlag = false;// 允许e后面有一个'+'/'-'
            numFlag = false;// e后面一定要有 数字
        } else {//其他情况都为非法数字形式。
            return false;
        }
    }
    return numFlag;
}
```

#### 复杂度分析

> 时间复杂度：O\(n\)
>
> 空间复杂度：O\(1\)

## 3.参考

* [https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

