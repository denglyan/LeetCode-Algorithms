## Merge Two Sorted Lists

### Problem

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

Example:

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

**Related Topics:**

`Linked List`

### Analysis

方法一：递归。

方法二：非递归，设置节点，跟踪判断、更新。

### Code

**递归**

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun mergeTwoLists(l1: ListNode?, l2: ListNode?): ListNode? {

        return when {
            l1 == null -> l2
            l2 == null -> l1
            l1.`val` < l2.`val` -> {
                l1.next = mergeTwoLists(l1.next, l2)
                l1
            }
            else -> {
                l2.next = mergeTwoLists(l1, l2.next)
                l2
            }
        }
    }
}
```

**非递归**

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun mergeTwoLists(l1: ListNode?, l2: ListNode?): ListNode? {

        val head = ListNode(0)
        var main = head
        var left = l1
        var right = l2

        loop@ while (true) {
            when {
                left == null -> {
                    main.next = right
                    break@loop
                }
                right == null -> {
                    main.next = left
                    break@loop
                }
                left.`val` < right.`val` -> {
                    main.next = left
                    left = left.next
                }
                else -> {
                    main.next = right
                    right = right.next
                }
            }
            main = main.next!!
        }

        return head.next
    }
}
```