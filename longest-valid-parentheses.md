## Longest Valid Parentheses

### Problem

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

For `"(()"`, the longest valid parentheses substring is `"()"`, which has length = 2.

Another example is `")()())"`, where the longest valid parentheses substring is `"()()"`, which has length = 4.

**Related Topics:**

`String` `Dynamic Programming`

### Analysis

方法一：动态规划，建立数组，遍历的同时，记录有效长度。只有当遇到 `)` 时，才可能存在有效字符串，所以仅在遇到 `)` 时进行判断。
- 当遇到 `()` 的情况时，其自身长度为 `2`，同时需要加上之前存在的有效长度 `dp[i-2]`。
- 当遇到 `))` 的情况时，首先需要确定后一个 `)` 是否有对应的 `(` 存在。若存在，其自身 `2`，加上 `(` 之前的有效长度 `dp[i - 2 - dp[i - 1]]`，以及其内部所包含的长度 `dp[i - 1]`。

方法二：方法一是有效长度的累加，而方法二则是字符串两端相减，来得到长度。利用栈来实现，遇到 `(` 时，将下标压入栈内，当遇到 `)` 时，弹出栈顶元素，其与下个元素的差值，便是有效长度。

方法三：扫描，设置两个变量 `left`，`right` 来统计遇到的 `(` 和 `)` 的数量。
- 当 `left == right` 时，为有效字符串，其长度为 `left * 2`。
- 当 `left < right` 时，必不可能形成有效字符串，重置 `left`，`right`。
- 由于无法判断 `left > right` 的情况，所以需要再来一次逆向扫描。

### Code

**累加：**

```kotlin
class Solution {

    fun longestValidParentheses(s: String): Int {

        val dp = IntArray(s.length)
        var max = 0

        for (i in 1 until s.length) {
            dp[i] = when {
                s[i] == ')' && s[i - 1] == '(' ->
                    (if (i >= 2) dp[i - 2] else 0) + 2
                s[i] == ')' && i - 1 >= dp[i - 1] && s[i - 1 - dp[i - 1]] == '(' ->
                    dp[i - 1] + (if (i - 2 >= dp[i - 1]) dp[i - 2 - dp[i - 1]] else 0) + 2
                else -> 0
            }
            max = maxOf(max, dp[i])
        }

        return max
    }
}
```

**相减：**

```kotlin
class Solution {

    fun longestValidParentheses(s: String): Int {

        val stack = java.util.Stack<Int>()
        stack.push(-1)

        var max = 0
        for (i in s.indices) {

            when {
                s[i] == '(' -> stack.push(i)
                else -> {
                    stack.pop()
                    when {
                        stack.empty() -> stack.push(i)
                        else -> max = maxOf(max, i - stack.peek())
                    }
                }
            }
        }

        return max
    }
}
```

**扫描：**

```kotlin
class Solution {

    fun longestValidParentheses(s: String): Int {

        return maxOf(s.scan(true), s.scan(false))
    }

    private fun String.scan(leftToRight: Boolean): Int {

        var left = 0
        var right = 0
        var max = 0
        var index = if (leftToRight) 0 else this.lastIndex
        val step = if (leftToRight) 1 else -1

        while (index < this.length && index > -1) {

            when (this[index]) {
                '(' -> left++
                ')' -> right++
            }

            when {
                left == right -> max = maxOf(max, left)
                (leftToRight && left < right) || (!leftToRight && left > right) -> {
                    left = 0
                    right = 0
                }
            }

            index += step
        }

        return max * 2
    }
}
```