## Longest Palindromic Substring

### Problem

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example 1:**

```
Input: "babad"

Output: "bab"

Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: "cbbd"

Output: "bb"
```

**Related Topics:**

`String`

### Analysis

**最长公共子串：**

原字符串与颠倒的字符串，求最长公共子串。但最长公共子串不一定是最长回文子串，例如 `abghba`，所以需要额外判断公共子串是否是回文子串。
原字符串与颠倒的字符串，分别以 `i`，`ri` 从两端开始，在原字符串上遍历。这样当公共子串在原字符串的同一位置时，便是回文子串。

**中心扩散：**

遍历字符串，以每一个字符或两个字符为中心（奇偶性），向外扩散搜索。

**Manacher's Algorithm：**

核心思想，记录回文串，减少重复判断：

1. 重构字符串，例如将 `abbc` 重构为 `[#a#b#b#c#]`，`#` 让字符串的奇偶性得到统一，`[]` 在字符串前后添加两个不同的字符，就不用担心越界问题（这里要保证，`#`，`[`，`]` 三个字符在原字符串中不存在）。
2. 设置数组 `p[i]`，遍历时，记录以 `i` 下标的字符为中心，其回文串半径。
3. 遍历时，记录并实时更新回文串所能达到的最右端下标 `maxRight`，以及其对应的中心 `maxRightIndex`。
4. 当 `i < maxRight` 时，以 `maxRightIndex` 为对称轴，存在 `j`。若以 `j` 为中心存在回文串，那么对称，以 `i` 为中心，同样存在回文串，即 `p[i] = p[j]` ，就可以减少此半径内字符的判断，直接从该范围的边界开始扩展。
5. 这里要注意：当 `p[j]` 的半径大于 `maxRight - i`，由于 `maxRight` 是所能预估的最右端，所以只能从 `maxRight` 开始向外遍历。

### Code

**最长公共子串：**

```kotlin
class Solution {

    fun longestPalindrome(s: String): String {

        var index = 0
        var maxLen = 0
        val len = Array(s.length + 1) { IntArray(s.length + 1) }

        var j: Int
        for (i in 1..s.length) {
            for (ri in s.length downTo 1) {
                if (s[i - 1] == s[ri - 1]) {
                    j = s.length - ri + 1
                    len[i][j] = len[i - 1][j - 1] + 1
                    if (maxLen < len[i][j] && i - ri + 1 == len[i][j]) {
                        maxLen = len[i][j]
                        index = i - 1
                    }
                }
            }
        }

        val result = StringBuilder()
        while (maxLen-- > 0) {
            result.insert(0, s[index--])
        }

        return result.toString()
    }
}
```

**中心扩散：**

```kotlin
class Solution {

    fun longestPalindrome(s: String): String {

        var index = 0
        var maxLen = 0
        var len: Int

        for (i in s.indices) {
            len = maxOf(getLen(s, i, i), getLen(s, i, i + 1))
            if (maxLen < len) {
                maxLen = len
                index = i - (len - 1) / 2
            }
        }

        return s.substring(index, index + maxLen)
    }

    private fun getLen(s: String, left: Int, right: Int): Int {

        var l = left
        var r = right

        while (l >= 0 && r < s.length && s[l] == s[r]) {
            l--
            r++
        }

        return r - l - 1
    }
}
```

**Manacher's Algorithm：**

```kotlin
class Solution {

    fun longestPalindrome(s: String): String {

        val ns = CharArray(s.length * 2 + 3)
        ns[0] = '['
        for (i in s.indices) {
            ns[i * 2 + 1] = '#'
            ns[i * 2 + 2] = s[i]
        }
        ns[s.length * 2 + 1] = '#'
        ns[s.length * 2 + 2] = ']'

        var maxLen = 0
        var maxIndex = 0
        var maxRight = 0
        var maxRightIndex = 0
        val p = IntArray(ns.size)

        for (i in 1 until ns.size - 1) {

            p[i] = if (i >= maxRight) 1 else minOf(maxRight - i, p[2 * maxRightIndex - i])
            while (ns[i - p[i]] == ns[i + p[i]]) {
                p[i]++
            }

            if (maxRight < p[i] + i) {
                maxRight = p[i] + i
                maxRightIndex = i
            }

            if (maxLen < p[i]) {
                maxLen = p[i]
                maxIndex = i
            }
        }
        
        return s.substring((maxIndex - 1) / 2 - (maxLen - 1) / 2, (maxIndex - 1) / 2 + maxLen / 2)
    }
}
```