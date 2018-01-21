## Remove Nth Node From End of List

### Problem

Given a linked list, remove the $$n^{th}$$ node from the end of list and return its head.

For example

```
   Given linked list: 1->2->3->4->5, and n = 2.

   After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**
Given n will always be valid.
Try to do this in one pass.

**Related Topics:**

`Linked List` `Two Pointers`

### Analysis

设置两个点，其间隔为 `n`，之后同步移动点 `front` 与 `back`，当 `front` 到达末尾时，`back` 正好达到操作位置。

### Code

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun removeNthFromEnd(head: ListNode?, n: Int): ListNode? {

        val nhead: ListNode? = ListNode(0)
        nhead?.next = head

        var front = nhead
        for (i in 1..n) {
            front = front?.next
        }

        var back = nhead
        while (front?.next != null) {
            front = front.next
            back = back?.next
        }

        front = back?.next
        back?.next = back?.next?.next
        front?.next = null

        return nhead?.next
    }
}
```