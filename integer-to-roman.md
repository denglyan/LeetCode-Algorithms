## Integer to Roman

Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

**Related Topics:**

`Math` `String`

### 描述

给出一个数字，转化为罗马数字

### 分析

方法一：打表列出每位上 0 - 9 对应的罗马数字

方法二：每位上，1 - 3 是 1 的累加，4 是 5 前面放个 1（相当于 5 - 1 = 4），6 - 8 是在 5 的基础上，1 的累加，9 则与 4 类似。1 - 3 和 6 - 8 有一定的共性，所以将 1，4，5，9 打表后，可以方便通过相减推出其它罗马数

### 代码

**简单打表**

```java
class Solution {

    String[] I = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
    String[] X = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
    String[] C = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
    String[] M = {"", "M", "MM", "MMM"};

    public String intToRoman(int num) {

        return M[num / 1000] + C[(num % 1000) / 100] + X[(num % 100) / 10] + I[num % 10];
    }
}
```

**相减取值**

```java
class Solution {

    int[] inum = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    String[] rnum = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    public String intToRoman(int num) {

        StringBuilder stringBuilder = new StringBuilder();

        int i = 0;
        while (num > 0) {
            while (num >= inum[i]) {
                num -= inum[i];
                stringBuilder.append(rnum[i]);
            }
            i++;
        }

        return stringBuilder.toString();
    }
}
```