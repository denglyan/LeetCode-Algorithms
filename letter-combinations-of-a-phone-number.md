## Letter Combinations of a Phone Number

### Problem

Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.

![](/assets/letter-combinations-of-a-phone-number.png)

```
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**Note:**
Although the above answer is in lexicographical order, your answer could be in any order you want.

**Related Topics:**

`String` `Backtracking`

### Analysis

方法一：通过函数递归调用，进行字符串拼接。

方法二：利用队列的先进先出，进行字符串拼接。

### Code

**递归**

```kotlin
class Solution {

    private var digits: String = ""
    private val map = make()
    private var list = mutableListOf<String>()

    fun letterCombinations(digits: String): List<String> {

        this.digits = digits
        list.clear()

        if (digits.isEmpty()) {
            return list
        }

        combinations(0, "")
        return list
    }

    private fun combinations(index: Int, result: String) {

        if (digits.length == index) {
            list.add(result)
            return
        }

        val array: Array<String> = map[digits[index]]!!
        for (s in array) {
            combinations(index + 1, result + s)
        }
    }

    private fun make(): Map<Char, Array<String>> {

        val map = HashMap<Char, Array<String>>()
        map.put('2', arrayOf("a", "b", "c"))
        map.put('3', arrayOf("d", "e", "f"))
        map.put('4', arrayOf("g", "h", "i"))
        map.put('5', arrayOf("j", "k", "l"))
        map.put('6', arrayOf("m", "n", "o"))
        map.put('7', arrayOf("p", "q", "r", "s"))
        map.put('8', arrayOf("t", "u", "v"))
        map.put('9', arrayOf("w", "x", "y", "z"))

        return map
    }
}
```

**队列**

```kotlin
import java.util.*

class Solution {

    private val map = arrayOf("abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz")

    fun letterCombinations(digits: String): List<String> {

        val list = LinkedList<String>()
        if (digits.isEmpty()) {
            return list
        }

        var s: String
        for (i in 0 until digits.length) {
            while (list.peek()?.length ?: 0 == i) {

                s = list.poll() ?: ""
                for (c in map[digits[i] - '2']) {
                    list.add(s + c)
                }
            }
        }

        return list
    }
}
```