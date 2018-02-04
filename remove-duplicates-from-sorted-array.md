## Remove Duplicates from Sorted Array

### Problem

Given a sorted array, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

**Example:**

```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the new length.
```

**Related Topics:**

`Array` `Two Pointers`

### Analysis

设置两个指标，一前一后：
- 当重复时，前动，后不动。
- 不重复时，前后都动，并且更新后指标的值。

### Code

```kotlin
class Solution {

    fun removeDuplicates(nums: IntArray): Int {

        var ri = 0
        for (i in 1 until nums.size) {
            if (nums[ri] != nums[i]) {
                ri++
                nums[ri] = nums[i]
            }
        }

        return ri + 1
    }
}
```