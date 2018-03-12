## Search in Rotated Sorted Array

### Problem

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

**Related Topics:**

`Array` `Binary Search`

### Analysis

方法一：先找出最小值所在的下标，确定目标在哪一边，然后进行二分查找。

方法二：当 `target < nums[mid]` 时，存在三种情况。
- 两则都在分割线（最小值与最大值之间）的左边：为升序，`target` 在 `nums[mid]` 左边，所以 `nums[mid]` 右边的元素舍弃，即 `end = mid - 1`。
- 两则都在分割线的右边：为升序，同上，`end = mid - 1`。
- 由于分割线左边元素大于右边，所以 `target` 在右边，`nums[mid]` 在左边：舍弃 `nums[mid]` 左边元素，即 `start = mid + 1`。

同理，`target > nums[mid]` 时，也有三种情况，处理方法类似。

### Code

**找最小值：**

```kotlin
class Solution {

    fun search(nums: IntArray, target: Int): Int {

        if (nums.isEmpty()) return -1

        val minIndex = findMin(nums)
        var start: Int
        var end: Int
        var mid: Int

        when {
            target > nums[nums.lastIndex] -> {
                start = 0
                end = minIndex - 1
            }
            else -> {
                start = minIndex
                end = nums.lastIndex
            }
        }

        while (start <= end) {
            mid = (start + end) / 2
            when {
                target == nums[mid] -> return mid
                target < nums[mid] -> end = mid - 1
                target > nums[mid] -> start = mid + 1
            }
        }

        return -1
    }

    private fun findMin(nums: IntArray): Int {

        var start = 0
        var end = nums.lastIndex
        var mid: Int

        while (start < end) {
            mid = (start + end) / 2
            when {
                nums[mid] > nums[end] -> start = mid + 1
                else -> end = mid
            }
        }

        return start
    }
}
```

**所有情况判断：**

```kotlin
class Solution {

    fun search(nums: IntArray, target: Int): Int {

        var start = 0
        var end = nums.lastIndex
        var mid: Int

        while (start <= end) {

            mid = (start + end) / 2

            when {
                target > nums[end] && nums[mid] <= nums[end] -> end = mid - 1
                target <= nums[end] && nums[mid] > nums[end] -> start = mid + 1
                else -> {
                    when {
                        target < nums[mid] -> end = mid - 1
                        target > nums[mid] -> start = mid + 1
                        else -> return mid
                    }
                }
            }
        }

        return -1
    }
}
```