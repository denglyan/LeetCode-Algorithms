## Longest Substring Without Repeating Characters

Given a string, find the length of the **longest substring** without repeating characters.

**Examples:**

```
Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**Related Topics:**

`Hash Table` `Two Pointers` `String`

### 描述

给出一个字符串，求出最长子串（子串内不存在重复的字符）的长度

### 分析

- 方法一：遍历字符串，进行计数 `len`，记录每个字符最新出现的位置 `index`，上一次进行操作的位置 `li`。若出现重复的字符，则 `len -= (index - li)`，这里要注意的是，判断 `index - li` 的正负。如果字符范围是确定的，可以采用数组来存储
- 方法二：大致思路和方法一类似，区别是，不采用计数，而是根据区间来，同时也要注意上一次进行操作的位置

### 代码

**计数**

```java
import java.util.HashMap;

public class Solution {

    public int lengthOfLongestSubstring(String s) {

        HashMap<Character, Integer> hashMap = new HashMap<Character, Integer>();

        int len = 0;
        int maxlen = 0;
        Integer key;

        for (int i = 0, li = 0, index; i < s.length(); i++) {

            len++;

            key = hashMap.get(s.charAt(i));
            index = (key != null ? key : 0);

            if (index != 0 && index - li > 0) {

                len -= (index - li);
                li = index;
            }

            hashMap.put(s.charAt(i), i + 1);

            if (maxlen < len) {
                maxlen = len;
            }
        }

        return maxlen;
    }
}
```

**区间**

```java
import java.util.HashMap;

public class Solution {

    public int lengthOfLongestSubstring(String s) {

        HashMap<Character, Integer> hashMap = new HashMap<Character, Integer>();

        int maxlen = 0;

        for (int i = 0, li = 0; i < s.length(); i++) {

            if (hashMap.containsKey(s.charAt(i))) {
                li = Math.max(hashMap.get(s.charAt(i)), li);
            }

            maxlen = Math.max(maxlen, i - li + 1);
            hashMap.put(s.charAt(i), i + 1);
        }

        return maxlen;
    }
}
```