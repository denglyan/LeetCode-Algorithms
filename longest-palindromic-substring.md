## Longest Palindromic Substring

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example 1:**

```
Input: "babad"

Output: "bab"

Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: "cbbd"

Output: "bb"
```

**Related Topics:**

`String`

### 描述

给出一个字符串，求出最长的回文子串

### 分析

**方法一：**

两个字符串（原字符串，颠倒后的字符串），求最长公共子串

这里有一个陷阱，例如字符串  `abghba`，`ab` 是得到的最长公共子串，但却不是回文子串。这里只需判断子串在字符串（原字符串，颠倒后的字符串）上是否处于同一位置，即可知道是否为回文串

**方法二：**

遍历字符串，以每一个字符或两个字符为中心（奇偶性），向外扩散搜索

**方法三：**

核心思想：遍历字符串的同时，记录其回文子串的状态，减少重复判断，从而达到线性的时间复杂度

首先重构字符串，例如将 `abbc` 重构为 `[#a#b#b#c#]`，`#` 让字符串的奇偶性得到统一，`[]` 在字符串前后添加两个不同的字符，就可以不用显示判断操作是否越界（这里要保证，`#`，`[`，`]` 三个字符在原字符串中不存在）

建立数组 `p[]`，用来存放新字符串的回文子串半径，例如， `#a#b`，`#` 的半径为 `1`，`a` 的半径为 `2`

`i` 为当前处于的位置
`maxRight` 代表目前已遍历的回文子串中，所能达到的最右端位置（` maxRight = p[i] + i`，为开区间）
`maxRightIndex` 代表其回文子串的对称轴

所以当 `i < maxRight` 时，可知，关于对称轴对应的左边，存在 `j`，与其回文子串状态相似
当 `p[j] < maxRight - i`，回文子串完全相对应，即 `p[i] = p[j]`
当 `p[j] >= maxRight - i`，`i` 的回文子串所能保证的仅仅是 `maxRight - i` 部分
综合处理，当 `i < maxRight` 时，`p[i] = Math.min(maxRight - i, p[j])`

当 `i >= maxRight` 时，没有参照，状态不明，所以 `p[i] = 1`，只能通过遍历判断回文状态

### 代码

**最长公共子串**

```java
public class Solution {

    public String longestPalindrome(String s) {

        int index = 0;
        int maxLen = 0;
        int len = s.length() + 1;
        int[][] n = new int[len][len];

        for (int i = 1; i < len; i++) {
            for (int j = 1; j < len; j++) {

                if (s.charAt(i - 1) == s.charAt(len - j - 1)) {

                    n[i][j] = n[i - 1][j - 1] + 1;
                    if (maxLen < n[i][j] && (i + j - len + 1) == n[i][j]) {
                        maxLen = n[i][j];
                        index = i;
                    }
                }
            }
        }

        String substring = "";
        while (maxLen > 0) {

            substring += s.charAt(--index);
            maxLen--;
        }
        return substring;
    }
}
```

**中心扩散**

```
public class Solution {

    public String longestPalindrome(String s) {

        int index = 0;
        int maxLen = 0;
        for (int i = 0; i < s.length(); i++) {

            int xlen = getLen(s, i, i);
            int ylen = getLen(s, i, i + 1);
            int len = Math.max(xlen, ylen);
            if (maxLen < len) {
                maxLen = len;
                index = i - (len - 1) / 2;
            }
        }

        return s.substring(index, index + maxLen);
    }

    private int getLen(String s, int left, int right) {

        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }

        return (right - 1) - (left + 1) + 1;
    }
}
```

**Manacher's Algorithm**

```
public class Solution {

    public String longestPalindrome(String s) {

        char[] newString = new char[s.length() * 2 + 3];

        newString[0] = '[';
        for (int i = 0, k = 1; i < s.length(); i++) {
            newString[k++] = '#';
            newString[k++] = s.charAt(i);
        }
        newString[s.length() * 2 + 1] = '#';
        newString[s.length() * 2 + 2] = ']';

        int maxLen = 0;
        int maxIndex = 0;
        int maxRight = 0;
        int maxRightIndex = 0;
        int[] p = new int[newString.length];

        int nslen = newString.length - 1;
        for (int i = 1; i < nslen; i++) {

            p[i] = i >= maxRight ? 1 : Math.min(maxRight - i, p[2 * maxRightIndex - i]);
            while (newString[i - p[i]] == newString[i + p[i]]) {
                p[i]++;
            }

            if (maxRight < p[i] + i) {
                maxRight = p[i] + i;
                maxRightIndex = i;
            }

            maxLen = Math.max(maxLen, p[i]);
            maxIndex = maxLen == p[i] ? i : maxIndex;
        }

        return new String(newString, maxIndex - maxLen + 1, maxLen * 2 - 1).replace("#", "");
    }
}
```