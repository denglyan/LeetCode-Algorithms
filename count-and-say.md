## Count and Say

### Problem

The count-and-say sequence is the sequence of integers with the first five terms as following:

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2, then one 1"` or `1211`.
Given an integer `n`, generate the `n^{th}` term of the count-and-say sequence.

Note: Each term of the sequence of integers will be represented as a string.

**Example 1:**

```
Input: 1
Output: "1"
```

**Example 2:**

```
Input: 4
Output: "1211"
```

**Related Topics:**

`String`

### Analysis

遍历并统计数字。

### Code

```kotlin
class Solution {

    fun countAndSay(n: Int): String {

        var string = "1"
        var stringBuilder: StringBuilder
        var count: Int

        (2..n).forEach {

            stringBuilder = StringBuilder()
            count = 1

            (0 until string.length - 1).forEach {
                when {
                    string[it] == string[it + 1] -> count++
                    else -> {
                        stringBuilder.append(count).append(string[it])
                        count = 1
                    }
                }
            }
            stringBuilder.append(count).append(string.last())

            string = stringBuilder.toString()
        }

        return string
    }
}
```