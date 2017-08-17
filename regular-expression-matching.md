## Regular Expression Matching

Implement regular expression matching with support for `.` and `*`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") ? false
isMatch("aa","aa") ? true
isMatch("aaa","aa") ? false
isMatch("aa", "a*") ? true
isMatch("aa", ".*") ? true
isMatch("ab", ".*") ? true
isMatch("aab", "c*a*b") ? true
```

**Related Topics:**

`Dynamic Programming` `Backtracking` `String`

### 描述

给出匹配的标准：`.` 匹配任何单一字符，`*` 匹配零个或多个上一个字符。编写函数，判断两个字符串是否匹配

### 分析

若不存在 `*`，则两个字符串只要同时遍历比较即可，所以这里重点关注 `*`

当遍历到 `*` 时，判断 `p` 的前一个字符与 `s` 当前字符：
- 若相等，则判断 `s` 的下一个字符
- 若不等，则判断 `p` 的下一个字符

仅仅这样考虑会忽略掉很多可能性，例如 `aaas` 和 `a*as`，若 `*` 延伸到最后一个 `a`，则会匹配出错。进行回溯，能够发现，当 `*` 匹配第二个 `a` 时，整个字符串是匹配的

第一种方法，递归。比较当前的字符，以及递归之后的字符串，在递归的过程中，遍历了所有的可能

第二种方法，DP。以 `f[][]` 二维数组，在遍历的过程中， 记录之前遍历过的字符串的匹配结果。在两个字符比较时，判断之前字符串的情况。相当于，已知遍历过的字符串是否匹配，加入一个新字符，判断新组成的字符串是否匹配，一步一步扩展到字符串末尾

### 代码

**递归**

```java
public class Solution {

    public boolean isMatch(String s, String p) {

        if (p.isEmpty()) {
            return s.isEmpty();
        }

        if (p.length() > 1 && '*' == p.charAt(1)) {
            return (elementMatch(s, p) && isMatch(s.substring(1), p))
                    || isMatch(s, p.length() > 2 ? p.substring(2) : "");
        } else {
            return elementMatch(s, p) && isMatch(s.substring(1), p.substring(1));
        }
    }

    private boolean elementMatch(String s, String p) {
        return !s.isEmpty() && (s.charAt(0) == p.charAt(0) || '.' == p.charAt(0));
    }
}
```

**Dynamic Programming**

```java
public class Solution {

    public boolean isMatch(String s, String p) {

        int sn = s.length();
        int pn = p.length();
        s = " " + s;
        p = " " + p;

        boolean[][] f = new boolean[sn + 1][pn + 1];

        f[0][0] = true;
        for (int i = 1; i <= sn; i++) {
            f[i][0] = false;
        }

        for (int j = 1; j <= pn; j++) {
            f[0][j] = p.charAt(j) == '*' && j > 1 && f[0][j - 2];
        }

        for (int i = 1; i <= sn; i++) {
            for (int j = 1; j <= pn; j++) {
                if (p.charAt(j) == '*') {
                    f[i][j] = (f[i - 1][j] && elementMatch(s, i, p, j - 1))
                            || (j > 1 && f[i][j - 2]);
                } else {
                    f[i][j] = f[i - 1][j - 1] && elementMatch(s, i, p, j);
                }
            }
        }

        return f[sn][pn];
    }

    private boolean elementMatch(String s, int i, String p, int j) {
        return p.charAt(j) == s.charAt(i) || p.charAt(j) == '.';
    }
}
```