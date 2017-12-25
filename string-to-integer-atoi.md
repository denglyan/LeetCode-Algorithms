## String to Integer (atoi)

### Problem

Implement atoi to convert a string to an integer.

**Hint:** Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

**Notes:** It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

**Requirements for atoi:**

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.

**Related Topics:**

`Math` `String`

### Analysis

- 方法一：通过正则筛选出字符串，然后使用系统函数进行转化。
- 方法二：参考 `Integer.parseInt()` 方法，在其基础上添加限制条件。

### Code

**正则**

```kotlin
class Solution {

    fun myAtoi(str: String): Int {

        return try {
            val s = str.trim().split(Regex("[^0-9+-]"), 2)[0]

            when {
                s.length > 11 -> if (s[0] == '-') Int.MIN_VALUE else Int.MAX_VALUE
                s.toLong() < Int.MIN_VALUE -> Int.MIN_VALUE
                s.toLong() > Int.MAX_VALUE -> Int.MAX_VALUE
                else -> s.toInt()
            }

        } catch (e: Exception) {
            0
        }
    }
}
```

**类似 Integer.parseInt()**

```kotlin
class Solution {

    fun myAtoi(str: String): Int {

        var i = 0
        val len = str.length

        while (i < len && str[i] == ' ') {
            i++
        }

        var result = 0
        var negative = false
        var limit = -Integer.MAX_VALUE
        val multmin: Int
        var digit: Int

        if (i < len && str[i] < '0') {

            if (str[i] == '-') {
                negative = true
                limit = Integer.MIN_VALUE
            } else if (str[i] != '+') {
                return 0
            }

            i++
        }

        if (i == len) {
            return 0
        }

        multmin = limit / 10
        while (i < len) {

            digit = str[i++] - '0'
            if (digit < 0 || digit > 9) {
                break
            }

            if (result < multmin) {
                return if (negative) Int.MIN_VALUE else Int.MAX_VALUE
            }
            result *= 10

            if (result < limit + digit) {
                return if (negative) Int.MIN_VALUE else Int.MAX_VALUE
            }
            result -= digit
        }

        return if (negative) result else -result
    }
}
```