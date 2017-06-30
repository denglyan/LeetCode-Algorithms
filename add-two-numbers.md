## Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
```

![](/assets/add-two-numbers.svg)

**Related Topics:**

`Linked List` `Math`

### 描述

两个非负整数，分别以两个非空链表，按位倒序存储，求这两个数的和，并且也以链表形式输出

### 分析

类似于归并排序里合并两个数组

### 代码

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {

    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

        int n, k = 0;
        ListNode listNode = new ListNode(0);
        ListNode head = listNode;

        while (l1 != null && l2 != null) {

            n = l1.val + l2.val + k;
            k = n / 10;
            listNode.next = new ListNode(n % 10);
            listNode = listNode.next;
            l1 = l1.next;
            l2 = l2.next;
        }

        while (l1 != null) {

            n = l1.val + k;
            k = n / 10;
            listNode.next = new ListNode(n % 10);
            listNode = listNode.next;
            l1 = l1.next;
        }

        while (l2 != null) {

            n = l2.val + k;
            k = n / 10;
            listNode.next = new ListNode(n % 10);
            listNode = listNode.next;
            l2 = l2.next;
        }

        if (k != 0) {
            listNode.next = new ListNode(k);
        }

        return head.next;
    }
}
```