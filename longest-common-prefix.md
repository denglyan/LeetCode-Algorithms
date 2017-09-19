## Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

**Related Topics:**

`String`

### 描述

求最长公共前缀

### 分析

#### 水平扫描

![](/assets/longest-common-prefix-1.png)

#### 垂直扫描

以列为单位扫描，适用于存在某些长度很短的字符串的情况

#### 分治

将字符串划分为多组，每组各自比较后，再合并比较

![](/assets/longest-common-prefix-2.png)

#### 二分查找

选出长度最短的字符串，并以此为基准进行二分

![](/assets/longest-common-prefix-3.png)

#### 前缀树

建立前缀树，进行查询

![](/assets/longest-common-prefix-4.png)

![](/assets/longest-common-prefix-5.png)

![](/assets/longest-common-prefix-6.png)

### 代码

#### 水平扫描

```java
public class Solution {

    public String longestCommonPrefix(String[] strs) {

        if (strs == null || strs.length == 0) {
            return "";
        }

        int n = strs.length;
        String prefix = strs[0];

        for (int i = 1; i < n; i++) {
            while (!strs[i].startsWith(prefix)) {

                prefix = prefix.substring(0, prefix.length() - 1);

                if (prefix.isEmpty()) {
                    return prefix;
                }
            }
        }

        return prefix;
    }
}
```

#### 垂直扫描

```java
public class Solution {

    public String longestCommonPrefix(String[] strs) {

        if (strs == null || strs.length == 0) {
            return "";
        }

        for (int i = 0; i < strs[0].length(); i++) {

            char c = strs[0].charAt(i);

            for (int j = 1; j < strs.length; j++) {
                if (i == strs[j].length() || strs[j].charAt(i) != c) {
                    return strs[0].substring(0, i);
                }
            }
        }

        return strs[0];
    }
}
```

#### 分治

```java
public class Solution {

    public String longestCommonPrefix(String[] strs) {

        if (strs == null || strs.length == 0) {
            return "";
        }

        return longestCommonPrefix(strs, 0, strs.length - 1);
    }

    public String longestCommonPrefix(String[] strs, int l, int r) {

        if (l == r) {
            return strs[l];
        }

        int mid = (l + r) / 2;
        String left = longestCommonPrefix(strs, l, mid);
        String right = longestCommonPrefix(strs, mid + 1, r);

        return longestCommonPrefix(left, right);
    }

    public String longestCommonPrefix(String left, String right) {

        int min = Math.min(left.length(), right.length());

        for (int i = 0; i < min; i++) {
            if (left.charAt(i) != right.charAt(i)) {
                return left.substring(0, i);
            }
        }

        return left.substring(0, min);
    }
}
```

#### 二分查找

```java
public class Solution {

    public String longestCommonPrefix(String[] strs) {

        if (strs == null || strs.length == 0) {
            return "";
        }

        int minLen = Integer.MAX_VALUE;
        for (String str : strs) {
            minLen = Math.min(minLen, str.length());
        }

        int l = 1, r = minLen, len = 0;

        while (l <= r) {
            int mid = (l + r) / 2;
            if (isCommonPrefix(strs, mid)) {
                len = mid;
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }

        return strs[0].substring(0, len);
    }

    public boolean isCommonPrefix(String[] strs, int len) {

        String str = strs[0].substring(0, len);

        for (int i = 1; i < strs.length; i++) {
            if (!strs[i].startsWith(str)) {
                return false;
            }
        }

        return true;
    }
}
```

#### 前缀树

```java
public class Solution {

    public String longestCommonPrefix(String[] strs) {

        if (strs == null || strs.length == 0) {
            return "";
        }

        if (strs.length == 1) {
            return strs[0];
        }

        Trie trie = new Trie();
        for (int i = 0; i < strs.length; i++) {
            trie.insert(strs[i]);
        }

        return trie.searchLongestPrefix(strs[0]);
    }

    class TrieNode {

        private TrieNode[] links;
        private final int R = 26;
        private boolean isEnd;
        private int size = 0;

        public TrieNode() {
            links = new TrieNode[R];
        }

        public boolean containsKey(char c) {
            return links[c - 'a'] != null;
        }

        public TrieNode get(char c) {
            return links[c - 'a'];
        }

        public void put(char c, TrieNode node) {
            links[c - 'a'] = node;
            size++;
        }

        public void setEnd() {
            isEnd = true;
        }

        public boolean isEnd() {
            return isEnd;
        }

        public int getSize() {
            return size;
        }
    }

    class Trie {

        private TrieNode root;

        public Trie() {
            root = new TrieNode();
        }

        public void insert(String word) {

            TrieNode node = root;
            for (int i = 0; i < word.length(); i++) {

                char currentChar = word.charAt(i);

                if (!node.containsKey(currentChar)) {
                    node.put(currentChar, new TrieNode());
                }

                node = node.get(currentChar);
            }

            node.setEnd();
        }

        public String searchLongestPrefix(String str) {

            TrieNode node = root;
            StringBuffer prefix = new StringBuffer();

            for (int i = 0; i < str.length(); i++) {

                char c = str.charAt(i);

                if (node.containsKey(c) && (node.getSize() == 1) && (!node.isEnd())) {
                    prefix.append(c);
                    node = node.get(c);
                } else {
                    return prefix.toString();
                }
            }

            return prefix.toString();
        }
    }
}
```

