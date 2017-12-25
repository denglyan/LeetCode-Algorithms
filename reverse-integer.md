## Reverse Integer

### Problem

Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

```
Input: 123
Output:  321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

**Note:**

Assume we are dealing with an environment which could only hold integers within the 32-bit signed integer range. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

**Related Topics:**

`Math`

### Analysis

重点在于溢出处理，可以参考 `Integer.parseInt()` 方法，核心思想是：
- 统一采用负数来处理。
- 提前缩小区间范围，避免溢出。

### Code

```kotlin
class Solution {

    fun reverse(x: Int): Int {

        var xi = x
        var result = 0
        var negative = false
        var limit = -Int.MAX_VALUE
        val multmin: Int
        var digit: Int

        if (xi < 0) {
            negative = true
            limit = Int.MIN_VALUE
        }

        multmin = limit / 10
        while (xi != 0) {

            digit = if (negative) -(xi % 10) else xi % 10
            xi /= 10

            if (result < multmin) {
                return 0
            }
            result *= 10

            if (result < limit + digit) {
                return 0
            }
            result -= digit
        }

        return if (negative) result else -result
    }
}
```