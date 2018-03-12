## Implement strStr()

### Problem

Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

**Example 1:**

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**Related Topics:**

`Two Pointers` `String`

### Analysis

`Sunday algorithm`，核心思想：尽可能多的跳过字符，减少判断。

当主串与模式串不匹配时，匹配点变更为主串上，模式串末尾的下一位所对应的字符。然后在模式串上寻找该字符（从后往前寻找，确保不会出现遗漏的情况）：
- 若存在，以模式串上对应的字符为标准，移动模式串到匹配点。
- 若不存在，以模式串头为标准，移动模式串到匹配点。

由于需要在模式串上寻找字符及其位置，所以可以使用数组来存储每个字符所对应的位置。从前往后遍历模式串进行存储，自然后面相同的字符会覆盖前面的，即符合在模式串上寻找的条件。

### Code

```kotlin
class Solution {

    fun strStr(haystack: String, needle: String): Int {

        if (needle.isEmpty()) {
            return 0
        }

        val charIndex = IntArray(256)
        charIndex.fill(-1)
        for (i in needle.indices) {
            charIndex[needle[i].toInt()] = i
        }

        var index = 0
        val len = haystack.length - needle.length
        var hi: Int
        var ni: Int
        while (index <= len) {

            hi = index
            ni = 0
            while (hi < haystack.length && ni < needle.length && haystack[hi] == needle[ni]) {
                hi++
                ni++
            }

            when {
                ni == needle.length -> return index
                index >= len -> return -1
                else -> index += (needle.length - charIndex[haystack[index + needle.length].toInt()])
            }
        }

        return -1
    }
}
```