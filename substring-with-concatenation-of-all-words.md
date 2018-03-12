## Substring with Concatenation of All Words

### Problem

You are given a string, **s**, and a list of words, **words**, that are all of the same length. Find all starting indices of substring(s) in **s** that is a concatenation of each word in **words** exactly once and without any intervening characters.

For example, given:
**s**: `"barfoothefoobarman"`
**words**: `["foo", "bar"]`

You should return the indices: `[0,9]`.
(order does not matter).

**Related Topics:**

`Hash Table` `Two Pointers` `String`

### Analysis

方法一：遍历，以每一个字符为起始点，进行判断。

方法二：切割，滑动。

将字符串，按单词长度进行切割。由于每次都取一个单词长度，只要起始点在切割点上，其实都属于在同一组上遍历。因此，起始点只需遍历一个单词长度 `[0 , words[0].length)`。

在同一组上遍历时，可以设置左右两个滑动。右滑动来增加匹配的单词，当匹配数大于标准时，通过左滑动来削减单词，从而只需遍历一次，即可找出这一组所有符合条件的值。

### Code

**遍历：**

```kotlin
class Solution {

    fun findSubstring(s: String, words: Array<String>): List<Int> {

        val result = arrayListOf<Int>()
        val total = java.util.LinkedList<String>()

        val len = words[0].length
        var word: String
        var temp: Boolean
        for (i in 0..s.length - len) {

            if (i + len * words.size > s.length) break

            total += words
            for (j in 0 until words.size) {

                word = s.substring(i + len * j, i + len * (j + 1))
                temp = words.indices
                        .firstOrNull { word == words[it] }
                        ?.let { true }
                        ?: false

                if (temp) total.remove(word) else break
            }

            if (total.isEmpty()) result.add(i) else total.clear()
        }

        return result
    }
}
```

**切割滑动：**

```kotlin
class Solution {

    fun findSubstring(s: String, words: Array<String>): List<Int> {

        val result = arrayListOf<Int>()
        val wordMap = hashMapOf<String, Int>()
        val tempMap = hashMapOf<String, Int>()
        val wordLen = words[0].length
        val wordsLen = wordLen * words.size
        val limit = s.length - wordLen
        var tempWordRight: String
        var tempWordLeft: String
        var count: Int
        var left: Int
        var right: Int

        for (w in words) {
            wordMap[w] = (wordMap[w] ?: 0) + 1
        }

        for (i in 0 until wordLen) {

            if (i + wordsLen > s.length) break

            left = i
            right = left
            count = 0
            tempMap.clear()

            while (right <= limit) {

                tempWordRight = s.substring(right, right + wordLen)

                if (wordMap[tempWordRight] == null) {
                    left = right + wordLen
                    right = left
                    count = 0
                    tempMap.clear()
                } else {

                    tempMap[tempWordRight] = (tempMap[tempWordRight] ?: 0) + 1
                    right += wordLen
                    count++

                    if (wordMap[tempWordRight]!! < tempMap[tempWordRight]!!) {
                        while (true) {

                            tempWordLeft = s.substring(left, left + wordLen)
                            tempMap[tempWordLeft] = tempMap[tempWordLeft]!! - 1
                            count--
                            left += wordLen

                            if (tempWordLeft == tempWordRight) break
                        }
                    }
                }

                if (count == words.size) {
                    result.add(left)
                }
            }
        }

        return result
    }
}
```