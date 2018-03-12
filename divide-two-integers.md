## Divide Two Integers

### Problem

Divide two integers without using multiplication, division and mod operator.

If it is overflow, return MAX_INT.

**Related Topics:**

`Math` `Binary Search`

### Analysis

先通过移位来靠近目标数，再利用减法抵达。

### Code

```kotlin
class Solution {

    fun divide(dividend: Int, divisor: Int): Int {

        if ((divisor == 0) || (dividend == Int.MIN_VALUE && divisor == -1)) {
            return Int.MAX_VALUE
        }

        val sign: Boolean = when {
            dividend < 0 && divisor < 0 -> true
            dividend > 0 && divisor > 0 -> true
            else -> false
        }
        val dividendL = Math.abs(dividend.toLong())
        val divisorL = Math.abs(divisor.toLong())
        var temp = divisorL

        var count: Long = 1
        while (dividendL > temp) {
            count = count.shl(1)
            temp = temp.shl(1)
        }

        while (dividendL < temp) {
            count--
            temp -= divisorL
        }

        return when {
            sign -> count.toInt()
            else -> 0 - count.toInt()
        }
    }
}
```