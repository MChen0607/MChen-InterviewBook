# 05. 替换空格【字符串】

## 1.题目描述

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

**示例 1：**

> 输入：s = "We are happy." 输出："We%20are%20happy."

**限制：**

> 0 &lt;= s 的长度 &lt;= 10000

## 2.解题思路

### 2.1 方法一：字符串

> 遍历整个字符串，遇到空格时，向StringBuilder对象 加入"%20"，其他情况正常加入即可。最后将结果转换成String对象

#### 代码实现

```text
 public String replaceSpace(String s) {
     StringBuilder sb=new StringBuilder();
     for(int i=0;i<s.length();i++){
         if(s.charAt(i)==' '){
             sb.append("%20");
         }else{
             sb.append(s.charAt(i));
         }
     }
     return sb.toString();
 }
```

#### 复杂度分析

> 时间复杂度 O\(n\)
>
> 空间复杂度 O\(n\)

## 3.参考

* [https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

