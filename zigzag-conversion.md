## ZigZag Conversion

### Problem

The string **"PAYPALISHIRING"** is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: **"PAHNAPLSIIGYIR"**
Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string text, int nRows);
```

**convert("PAYPALISHIRING", 3)** should return **"PAHNAPLSIIGYIR"**.

**Related Topics:**

`String`

### Analysis

```
  [ 0]            [ 6]            [12]          
  [ 1]      [ 5]  [ 7]      [11]  [13]    
  [ 2]  [ 4]      [ 8]  [10]      [14]
  [ 3]            [ 9]            [15]          
```

可以发现：
- 每一完整的列，其差值为 `2 * numRows - 2`。
- 斜线左右分割这差值，其上下层之间，分割的差值 `±2`。

### Code

```kotlin
class Solution {

    fun convert(s: String, numRows: Int): String {

        if (numRows == 1) {
            return s
        }

        val dvalue = IntArray(2)
        dvalue[0] = 2 * numRows - 2

        val ns = CharArray(s.length)
        var index = 0
        var j: Int
        var df: Int

        val limit = minOf(s.length, numRows)
        for (i in 0 until limit) {

            j = i
            df = 1
            while (j < s.length) {

                ns[index++] = s[j]

                df = (df + 1) % 2
                df = if (dvalue[df] == 0) (df + 1) % 2 else df
                j += dvalue[df]
            }

            dvalue[0] -= 2
            dvalue[1] += 2
        }

        return String(ns)
    }
}
```