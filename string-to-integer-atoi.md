## String to Integer (atoi)

Implement atoi to convert a string to an integer.

**Related Topics:**

`Math` `String`

### 描述

实现 `atoi` 方法，字符串转化为整数

### 分析

首先了解 `atoi` 方法的定义，注意一些不同之处，实现可以参考 `Integer.parseInt()` 方法

### 代码

```java
public class Solution {

    public int myAtoi(String str) {

        if (str == null || str.length() == 0) {
            return 0;
        }

        int index = 0;
        while (str.charAt(index) == ' ' && index < str.length()) {
            index++;
        }

        int result = 0;
        boolean negative = false;
        int limit = -Integer.MAX_VALUE;
        int multmin;
        int digit;

        if (index < str.length()) {

            char firstChar = str.charAt(index);
            if (firstChar < '0') {
                if (firstChar == '-') {
                    negative = true;
                    limit = Integer.MIN_VALUE;
                } else if (firstChar != '+')
                    return 0;

                if (str.length() - index == 1)
                    return 0;

                index++;
            }

            multmin = limit / 10;
            while (index < str.length()) {

                digit = Character.digit(str.charAt(index++), 10);
                if (digit < 0) {
                    break;
                }

                if (result < multmin) {
                    return negative ? Integer.MIN_VALUE : Integer.MAX_VALUE;
                }
                result *= 10;

                if (result < limit + digit) {
                    return negative ? Integer.MIN_VALUE : Integer.MAX_VALUE;
                }
                result -= digit;
            }
        } else {
            return 0;
        }

        return negative ? result : -result;
    }
}
```