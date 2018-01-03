## Regular Expression Matching

### Problem

Implement regular expression matching with support for `.` and `*`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```

**Related Topics:**

`Dynamic Programming` `Backtracking` `String`

### Analysis

影响匹配的主要因素是 `*`：
- 当遇到 `*` 时，判断 `p` 前一个字符与 `s` 当前字符：
 - 若相等，`p` 不变，接着判断 `s` 的下一个字符。
 - 若不等，`s` 不变，接着判断 `p` 的下一个字符。
 
- 没有遇到 `*` 时，判断 `p` 当前字符与`s` 当前字符：
 - 若相等，判断 `p` 下一个字符与`s` 下一个字符。
 - 若不等，返回 `false`。

因为遍历时， `*` 扩展的数量是不确定的，例如：`aaas` 和 `a*as`，通常不会一步到位，需要涉及到回溯。

方法一：递归。遇到 `*` 时，两种情况都有可能得出正确结果，用 `||` 关联。没有遇到 `*` 时，只需判断一种情况。

方法二：DP。建立二维数组，遍历的同时，记录之前所有的匹配情况，依据这些情报，来确定当前的状态。

### Code

**递归**

```kotlin
class Solution {

    fun isMatch(s: String, p: String): Boolean {

        return isMatch(s, 0, p, 0)
    }

    private fun isMatch(s: String, si: Int, p: String, pi: Int): Boolean {

        return when {
            si >= s.length && pi >= p.length ->
                true
            pi + 1 < p.length && p[pi + 1] == '*' ->
                (si < s.length && (p[pi] == s[si] || p[pi] == '.') && isMatch(s, si + 1, p, pi)) || isMatch(s, si, p, pi + 2)
            else ->
                pi < p.length && si < s.length && (p[pi] == s[si] || p[pi] == '.') && isMatch(s, si + 1, p, pi + 1)
        }
    }
}
```

**Dynamic Programming**

```kotlin
class Solution {

    fun isMatch(s: String, p: String): Boolean {

        val ns = " " + s
        val np = " " + p

        val dp = Array(ns.length) { BooleanArray(np.length) }
        dp[0][0] = true

        for (i in 1 until ns.length) {
            dp[i][0] = false
        }

        for (i in 1 until np.length) {
            dp[0][i] = np[i] == '*' && i > 1 && dp[0][i - 2]
        }

        for (i in 1 until ns.length) {
            for (j in 1 until np.length) {
                dp[i][j] = when {
                    np[j] == '*' -> ((ns[i] == np[j - 1] || np[j - 1] == '.') && dp[i - 1][j]) || (j > 1 && dp[i][j - 2])
                    else -> (ns[i] == np[j] || np[j] == '.') && dp[i - 1][j - 1]
                }
            }
        }

        return dp[ns.lastIndex][np.lastIndex]
    }
}
```