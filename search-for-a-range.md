## Search for a Range

### Problem

Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return `[-1, -1]`.

**For example**,
Given `[5, 7, 7, 8, 8, 10]` and target value `8`,
return `[3, 4]`.

**Related Topics:**

`Array` `Binary Search`

### Analysis

方法一：首先通过二分查找确定目标，然后左右向外扩展搜索。

方法二：区间的左右两点都通过二分查找获取。

### Code

**向外扩展：**

```kotlin
class Solution {

    fun searchRange(nums: IntArray, target: Int): IntArray {

        val mid = binarySearch(nums, target)
        if (mid == -1) {
            return intArrayOf(-1, -1)
        }

        var left = mid
        while (left - 1 >= 0) {
            if (nums[left - 1] != target) {
                break
            }
            left--
        }

        var right = mid
        while (right + 1 < nums.size) {
            if (nums[right + 1] != target) {
                break
            }
            right++
        }

        return intArrayOf(left, right)
    }

    private fun binarySearch(nums: IntArray, target: Int): Int {

        var left = 0
        var right = nums.lastIndex
        var mid: Int

        while (left <= right) {

            mid = (left + right) / 2
            when {
                nums[mid] == target -> return mid
                nums[mid] < target -> left = mid + 1
                else -> right = mid - 1
            }
        }

        return -1
    }
}
```

**二分确定两端：**

```kotlin
class Solution {

    fun searchRange(nums: IntArray, target: Int): IntArray {

        val left = binarySearch(nums, target, true)
        if (left == nums.size || nums[left] != target) {
            return intArrayOf(-1, -1)
        }

        val right = binarySearch(nums, target, false) - 1

        return intArrayOf(left, right)
    }

    private fun binarySearch(nums: IntArray, target: Int, leftInterval: Boolean): Int {

        var left = 0
        var right = nums.size
        var mid: Int

        while (left < right) {

            mid = (left + right) / 2
            when {
                nums[mid] > target || (leftInterval && nums[mid] == target) -> right = mid
                else -> left = mid + 1
            }
        }

        return right
    }
}
```