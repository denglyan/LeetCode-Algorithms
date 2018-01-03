## Palindrome Number

### Problem

Determine whether an integer is a palindrome. Do this without extra space.

**Some hints:**
Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

**Related Topics:**

`Math`

### Analysis

将数截成两部分，一正一反，比较大小。由于没有前导零的存在，所以首先可以排除末尾有零的数。当然，负数由于带有符号位，所以也排除。

### Code

```kotlin
class Solution {

    fun isPalindrome(x: Int): Boolean {

        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false
        }

        var xi = x
        var reverse = 0
        while (xi > reverse) {
            reverse = reverse * 10 + xi % 10
            xi /= 10
        }

        return xi == reverse || xi == reverse / 10
    }
}
```