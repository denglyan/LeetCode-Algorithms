## Reverse Integer

Reverse digits of an integer.

```
Example1: x = 123, return 321
Example2: x = -123, return -321
```

**Note:**

The input is assumed to be a 32-bit signed integer. Your function should **return 0 when the reversed integer overflows**.

**Related Topics:**

`Math`

### 描述

整数倒叙，若溢出，则返回 0

### 分析

重点在于如何处理溢出，可以参考 `Integer.parseInt()` 方法，核心思想是：
- 统一采用负数来处理
- 缩小区间范围，以便判断是否溢出

### 代码

```java
public class Solution {

    public int reverse(int x) {

        int result = 0;
        boolean negative = false;
        int limit = -Integer.MAX_VALUE;

        if (x < 0) {
            negative = true;
            limit = Integer.MIN_VALUE;
        }

        int digit;
        int multmin = limit / 10;
        while (x != 0) {

            digit = Math.abs(x % 10);
            x /= 10;

            if (result < multmin) {
                return 0;
            }
            result *= 10;

            if (result < limit + digit) {
                return 0;
            }
            result -= digit;
        }

        return negative ? result : -result;
    }
}
```