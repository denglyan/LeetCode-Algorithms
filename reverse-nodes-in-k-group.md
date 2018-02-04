## Swap Nodes in Pairs

### Problem

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed.

Only constant memory is allowed.

For example,
Given this linked list: `1->2->3->4->5`

For k = 2, you should return: `2->1->4->3->5`

For k = 3, you should return: `3->2->1->4->5`

**Related Topics:**

`Linked List`

### Analysis

遍历 `k` 个后，设置标记，然后交换这 `k` 个数。完成后，回到标记点，继续往前遍历。

### Code

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun reverseKGroup(head: ListNode?, k: Int): ListNode? {

        val h = ListNode(0)
        h.next = head

        var node = h
        var left = h.next
        var right: ListNode?
        var temp: ListNode?

        while (left != null) {

            temp = left
            (1 until k).forEach { left = left?.next ?: return h.next }

            right = left?.next

            while (temp != left) {
                node.next = temp?.next
                temp?.next = right
                left?.next = temp
                right = temp
                temp = node.next
            }

            (1..k).forEach { node = node.next!! }
            left = node.next
        }

        return h.next
    }
}
```