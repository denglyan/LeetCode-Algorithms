## Merge k Sorted Lists

### Problem

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Related Topics:**

`Linked List` `Divide and Conquer` `Heap`

### Analysis

方法一：按顺序纵向遍历。

方法二：利用优先队列，减少每次查找最小值所耗费的时间。

方法三：分治，两两合并。

### Code

**纵向遍历：**

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun mergeKLists(lists: Array<ListNode?>): ListNode? {

        val head = ListNode(0)
        var node = head
        var min: Int
        var index: Int

        while (true) {

            min = Int.MAX_VALUE
            index = -1

            for (i in lists.indices) {
                if (lists[i] != null && min >= lists[i]!!.`val`) {
                    min = lists[i]!!.`val`
                    index = i
                }
            }

            if (index == -1) {
                break
            }

            node.next = lists[index]
            node = node.next!!
            lists[index] = lists[index]!!.next
        }

        return head.next
    }
}
```

**优先队列：**

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun mergeKLists(lists: Array<ListNode?>): ListNode? {

        val head = ListNode(0)
        var node = head

        val queue: java.util.Queue<ListNode> = java.util.PriorityQueue(kotlin.Comparator { o1, o2 -> o1.`val` - o2.`val` })
        lists.filterNotNullTo(queue)

        var temp: ListNode?
        while (queue.isNotEmpty()) {

            temp = queue.poll()
            node.next = temp
            node = node.next!!

            temp = temp.next
            if (temp != null) {
                queue.add(temp)
            }
        }

        return head.next
    }
}
```

**分治：**

```kotlin
/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {

    fun mergeKLists(lists: Array<ListNode?>): ListNode? {

        val size = lists.size
        var interval = 1

        while (interval < size) {
            for (i in 0 until size - interval step interval * 2) {
                lists[i] = merge2Lists(lists[i], lists[i + interval])
            }
            interval *= 2
        }

        return if (size > 0) lists[0] else null
    }

    private fun merge2Lists(x: ListNode?, y: ListNode?): ListNode? {

        return when {
            x == null -> y
            y == null -> x
            x.`val` < y.`val` -> {
                x.next = merge2Lists(x.next, y)
                x
            }
            else -> {
                y.next = merge2Lists(x, y.next)
                y
            }
        }
    }
}
```