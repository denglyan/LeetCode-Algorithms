## Add Two Numbers

### Problem

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
```

**Related Topics:**

`Linked List` `Math`

### Analysis

由于是倒序，更容易操作，每位对应相加，同时注意进位和判空，即可。

### Code

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun addTwoNumbers(l1: ListNode?, l2: ListNode?): ListNode? {

        var listNode1 = l1
        var listNode2 = l2

        var carry = 0
        var listNode: ListNode? = ListNode(0)
        val head = listNode

        while (listNode1 != null || listNode2 != null) {

            carry = (listNode1?.`val` ?: 0) + (listNode2?.`val` ?: 0) + carry / 10
            listNode?.next = ListNode(carry % 10)
            listNode = listNode?.next

            listNode1 = listNode1?.next
            listNode2 = listNode2?.next
        }

        carry /= 10
        if (carry != 0) {
            listNode?.next = ListNode(carry)
        }

        return head?.next
    }
}
```