## Generate Parentheses

### Problem

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

**Related Topics:**

`String` `Backtracking`

### Analysis

方法一：递归。

方法二：非递归。

```
f(0): ""
f(1): "("f(0)")"
f(2): "("f(0)")"f(1) , "("f(1)")"
f(3): "("f(0)")"f(2) , "("f(1)")"f(1) , "("f(2)")"
f(n): "("f(0)")"f(n-1) , "("f(1)")"f(n-2) , "("f(2)")"f(n-3) ... "("f(n-1)")"
```

### Code

**递归**

```kotlin
class Solution {

    private lateinit var list: MutableList<String>

    fun generateParenthesis(n: Int): List<String> {

        list = mutableListOf()
        generateParenthesis(n, 0, "")

        return list
    }

    private fun generateParenthesis(n: Int, stack: Int, s: String) {

        when {
            n < 0 || stack < 0 -> return
            n == 0 && stack == 0 -> list.add(s)
            else -> {
                generateParenthesis(n - 1, stack + 1, s + "(")
                generateParenthesis(n, stack - 1, s + ")")
            }
        }
    }
}
```

**非递归**

```kotlin
class Solution {

    fun generateParenthesis(n: Int): List<String> {

        val list = mutableListOf<List<String>>()
        list.add(listOf(""))

        var temp: MutableList<String>
        for (i in 1..n) {

            temp = mutableListOf()
            for (j in 0 until i) {
                for (first in list[j]) {
                    list[i - 1 - j].mapTo(temp) { "($first)$it" }
                }
            }
            list.add(temp)
        }

        return list[n]
    }
}
```