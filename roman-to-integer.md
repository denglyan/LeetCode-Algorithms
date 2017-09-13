## Roman to Integer

Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

**Related Topics:**

`Math` `String`

### 描述

给出一个罗马数，转换为一般数字

### 分析

通过观察，可以发现，每位上，`4` 和 `9` 是特别的，例如：`IV（4）`，`IX（9）`，`4` 可以看成是 `-1 + 5`，`9` 则是 `-1 + 10`，所以将每个罗马字符对应的数字进行累加即可（当前一个字符比后一个字符小，则看成负数相加）

### 代码

```java
import java.util.HashMap;

class Solution {

    private HashMap<Character, Integer> hashMap;

    public Solution() {

        hashMap = new HashMap<Character, Integer>();
        hashMap.put('M', 1000);
        hashMap.put('D', 500);
        hashMap.put('C', 100);
        hashMap.put('L', 50);
        hashMap.put('X', 10);
        hashMap.put('V', 5);
        hashMap.put('I', 1);
    }

    public int romanToInt(String s) {

        int result = 0;
        int pre = 0;
        for (int i = 0; i < s.length(); i++) {

            result += pre < hashMap.get(s.charAt(i)) ? -pre : pre;
            pre = hashMap.get(s.charAt(i));
        }

        return result + pre;
    }
}
```