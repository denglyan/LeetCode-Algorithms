## Longest Substring Without Repeating Characters

### Problem

Given a string, find the length of the **longest substring** without repeating characters.

**Examples:**

```
Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**Related Topics:**

`Hash Table` `Two Pointers` `String`

### Analysis

遍历字符串时，以当前字符为结尾，符合题意的字符串长度为：当前位置 - maxOf（上一个重复字符出现的位置，上一个当前字符出现的位置）

### Code

```kotlin
class Solution {

    fun lengthOfLongestSubstring(s: String): Int {

        val map = HashMap<Char, Int>()

        var last = -1
        var maxLen = 0

        for (i in s.indices) {

            last = maxOf(last, map[s[i]] ?: -1)
            maxLen = maxOf(maxLen, i - last)
            map[s[i]] = i
        }

        return maxLen
    }
}
```