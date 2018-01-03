## Integer to Roman

### Problem

Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

**Related Topics:**

`Math` `String`

### Analysis

方法一：打表，然后按位，对应进行转化。

方法二：相邻的重复字符，具有重复叠加的含义。打表记录下特殊的字符后，便可以通过相减，推出重复字符出现的次数，从而得到结果。

### Code

**简单打表**

```kotlin
class Solution {

    private val i = arrayOf("", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX")
    private val x = arrayOf("", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC")
    private val c = arrayOf("", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM")
    private val m = arrayOf("", "M", "MM", "MMM")

    fun intToRoman(num: Int): String {

        return m[num / 1000] + c[(num % 1000) / 100] + x[(num % 100) / 10] + i[num % 10]
    }
}
```

**相减取值**

```kotlin
class Solution {

    private val i = arrayOf(1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1)
    private val r = arrayOf("M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I")

    fun intToRoman(num: Int): String {

        val string = StringBuilder()

        var index = 0
        var n = num
        while (n > 0) {
            while (n >= i[index]) {
                n -= i[index]
                string.append(r[index])
            }
            index++
        }

        return string.toString()
    }
}
```