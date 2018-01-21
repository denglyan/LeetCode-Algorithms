## Valid Parentheses

### Problem

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

The brackets must close in the correct order, `"()"` and `"()[]{}"` are all valid but `"(]"` and `"([)]"` are not.

**Related Topics:**

`String` `Stack`

### Analysis

模拟栈，通过栈顶元素，来判断括号的对应关系。

### Code

```kotlin
class Solution {

    fun isValid(s: String): Boolean {

        val stack = CharArray(s.length + 1)

        var top = 0
        for (c in s) {

            when (when (c) {
                ')' -> '('
                ']' -> '['
                '}' -> '{'
                else -> '#'
            }) {
                '#' -> stack[++top] = c
                stack[top] -> top--
                else -> return false
            }
        }

        return top == 0
    }
}
```