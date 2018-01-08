## Longest Common Prefix

### Problem

Write a function to find the longest common prefix string amongst an array of strings.

**Related Topics:**

`String`

### Analysis

**水平扫描**

![](/assets/longest-common-prefix-1.png)

**垂直扫描**

以列为单位扫描，适用于存在短字符串的情况。

**分治**

将字符串划分为多组，每组各自比较后，再合并比较。

![](/assets/longest-common-prefix-2.png)

**长度二分**

以最短的字符串长度为基础，将长度进行二分，然后将第一个元素，按长度切割后比较。

![](/assets/longest-common-prefix-3.png)

**前缀树**

建立前缀树，进行查询。

![](/assets/longest-common-prefix-4.png)

![](/assets/longest-common-prefix-5.png)

![](/assets/longest-common-prefix-6.png)

### Code

**水平扫描**

```kotlin
class Solution {

    fun longestCommonPrefix(strs: Array<String>): String {

        if (strs.isEmpty()) {
            return ""
        }

        val prefix = strs[0]
        var len = prefix.length

        for (i in 1 until strs.size) {
            while (!strs[i].regionMatches(0, prefix, 0, len)) {
                len--
            }
        }

        return prefix.substring(0, len)
    }
}
```

**垂直扫描**

```kotlin
class Solution {

    fun longestCommonPrefix(strs: Array<String>): String {

        if (strs.isEmpty()) {
            return ""
        }

        for (i in 0 until strs[0].length) {
            (1 until strs.size)
                    .firstOrNull { i == strs[it].length || strs[0][i] != strs[it][i] }
                    ?.let { return strs[0].substring(0, i) }
        }

        return strs[0]
    }
}
```

**分治**

```kotlin
class Solution {

    fun longestCommonPrefix(strs: Array<String>): String {

        if (strs.isEmpty()) {
            return ""
        }

        return divideAndConquer(strs, 0, strs.size - 1)
    }

    private fun divideAndConquer(strs: Array<String>, left: Int, right: Int): String {

        if (left == right) {
            return strs[left]
        }

        val mid = (left + right) / 2
        return divideAndConquer(strs, left, mid).compare(divideAndConquer(strs, mid + 1, right))
    }

    private fun String.compare(s: String): String {

        val min = minOf(this.length, s.length)

        return (0 until min)
                .firstOrNull { this[it] != s[it] }
                ?.let { this.substring(0, it) }
                ?: this.substring(0, min)
    }
}
```

**长度二分**

```kotlin
class Solution {

    fun longestCommonPrefix(strs: Array<String>): String {

        if (strs.isEmpty()) {
            return ""
        }

        var minLen = Int.MAX_VALUE
        for (s in strs) {
            minLen = minOf(minLen, s.length)
        }

        var left = 0
        var right = minLen
        var mid: Int
        var len = 0

        while (left <= right) {

            mid = (left + right) / 2

            if (isCommonPrefix(strs, mid)) {
                len = mid
                left = mid + 1
            } else {
                right = mid - 1
            }
        }

        return strs[0].substring(0, len)
    }

    private fun isCommonPrefix(strs: Array<String>, len: Int): Boolean {

        (1 until strs.size)
                .firstOrNull { !strs[0].regionMatches(0, strs[it], 0, len) }
                ?.let { return false }
                ?: return true
    }
}
```

**前缀树**

```kotlin
class Solution {

    fun longestCommonPrefix(strs: Array<String>): String {

        if (strs.isEmpty()) {
            return ""
        }

        val trie = Trie()
        for (s in strs) {
            trie.insert(s)
        }

        return trie.searchLongestPrefix(strs[0])
    }

    class TrieNode {

        private val links: Array<TrieNode?> = arrayOfNulls(26)
        var isEnd: Boolean = false
        var size: Int = 0
            private set

        fun getTrieNode(c: Char): TrieNode? {
            return links[c - 'a']
        }

        fun setTrieNode(c: Char, node: TrieNode) {
            links[c - 'a'] = node
            size++
        }

        fun containNode(c: Char): Boolean {
            return links[c - 'a'] != null
        }
    }

    class Trie {

        private val root = TrieNode()

        fun insert(s: String) {

            var node = root

            for (c in s) {

                if (!node.containNode(c)) {
                    node.setTrieNode(c, TrieNode())
                }

                node = node.getTrieNode(c)!!
            }

            node.isEnd = true
        }

        fun searchLongestPrefix(s: String): String {

            var node = root
            val result = StringBuilder()

            for (c in s) {

                if (node.containNode(c) && node.size == 1 && !node.isEnd) {
                    result.append(c)
                    node = node.getTrieNode(c)!!
                } else {
                    return result.toString()
                }
            }

            return result.toString()
        }
    }
}
```