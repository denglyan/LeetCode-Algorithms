## Palindrome Number

Determine whether an integer is a palindrome. Do this without extra space.

**Related Topics:**

`Math`

### 描述

判断一个整数是否为回文

### 分析

若一个整数为回文，按对称轴截断成两部分，`x`，`y`，`x <= y`，则 `x = y || x = y / 10`（偶数位和奇数位）
有一个需要注意的点：例如 `100`，满足上述条件，但不是回文。这里可以排除掉 `x % 10 == 0 && x != 0` 这批数，因为没有前导 `0` 的存在，不可能构成回文

### 代码

```java
public class Solution {

    public boolean isPalindrome(int x) {

        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int reverse = 0;
        while (x > reverse) {
            reverse = reverse * 10 + x % 10;
            x /= 10;
        }

        return (x == reverse || x == reverse / 10);
    }
}
```