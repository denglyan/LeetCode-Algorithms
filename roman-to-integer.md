## Roman to Integer

### Problem

Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

**Related Topics:**

`Math` `String`

### Analysis

通过观察，可以发现，每位上，`4` 和 `9` 是特别的，例如：`IV（4）`，`IX（9）`，`4` 可以看成是 `-1 + 5`，`9` 则是 `-1 + 10`，所以将每个罗马字符对应的数字进行累加即可（当前一个字符比后一个字符小，则看成负数相加）。

### Code

```kotlin
class Solution {

    private val map = mapOf(
            'M' to 1000,
            'D' to 500,
            'C' to 100,
            'L' to 50,
            'X' to 10,
            'V' to 5,
            'I' to 1)

    fun romanToInt(s: String): Int {

        var num = 0
        var pre = 0
        for (i in s) {
            num += if (pre < map[i]!!) -pre else pre
            pre = map[i]!!
        }

        return num + pre
    }
}
```