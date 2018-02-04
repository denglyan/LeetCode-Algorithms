## Remove Element

### Problem

Given an array and a value, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example:**

```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.
```

**Related Topics:**

`Array` `Two Pointers`

### Analysis

顺序不变：设置两个标记点，一个用来遍历数组，过滤掉指定元素，将其余元素放置在另一标记点上。

顺序改变：遍历数组，将指定元素与数组最后一个元素交换，同时缩短数组长度。

### Code
