## Swap Nodes in Pairs

### Problem

Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given `1->2->3->4`, you should return the list as `2->1->4->3`.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

**Related Topics:**

`Linked List`

### Analysis

方法一：设置两个指标，遍历的同时进行更换。

方法二：递归。（不符合题意，题目要求只能使用恒定的空间）

### Code

**遍历：**

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun swapPairs(head: ListNode?): ListNode? {

        val h = ListNode(0)
        h.next = head

        var left: ListNode = h
        var right: ListNode? = left.next?.next

        while (right != null) {

            swapPair(left, right)
            left = left.next!!.next!!
            right = left.next?.next
        }

        return h.next
    }

    private fun swapPair(left: ListNode, right: ListNode) {

        left.next?.next = right.next
        right.next = left.next
        left.next = right
    }
}
```

**递归：**

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun swapPairs(head: ListNode?): ListNode? {

        val n = head?.next ?: return head

        head.next = swapPairs(n.next)
        n.next = head

        return n
    }
}
```