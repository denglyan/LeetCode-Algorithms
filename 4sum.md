## 4Sum

### Problem

Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

**Note:** The solution set must not contain duplicate quadruplets.

```
For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

**Related Topics:**

`Array` `Hash Table` `Two Pointers`

### Analysis

核心思想与 `3Sum` 相同。这里可以一般化，即求 `nSum`。通过取数，变更 `target`，来进行降级。将 `n` 层，降为 `n-1` 层，直至降为 `2` 层。

这里需要注意：在取数降级的过程中，需要判断相邻的数是否相同，避免结果重复。

### Code

```kotlin
class Solution {

    fun fourSum(nums: IntArray, target: Int): List<List<Int>> {

        val result = mutableListOf<List<Int>>()

        if (nums.isNotEmpty()) {
            nums.sort()
            nSum(nums, target, 4, 0, listOf(), result)
        }

        return result
    }

    private fun nSum(nums: IntArray, target: Int, n: Int, start: Int, temp: List<Int>, result: MutableList<List<Int>>) {

        if (nums[start] * n > target || nums.last() * n < target) {
            return
        }

        if (n == 2) {

            var left = start
            var right = nums.size - 1
            while (left < right) {

                when {
                    nums[left] + nums[right] == target -> {
                        result.add(temp + listOf(nums[left], nums[right]))
                        while (left < right && nums[left] == nums[++left]) {
                        }
                        while (left < right && nums[right] == nums[--right]) {
                        }
                    }
                    nums[left] + nums[right] < target -> left++
                    nums[left] + nums[right] > target -> right--
                }
            }

            return
        }

        var i = start
        while (i < nums.size - 2) {
            nSum(nums, target - nums[i], n - 1, i + 1, temp + nums[i], result)
            while (i < nums.size - 2 && nums[i] == nums[++i]) {
            }
        }
    }
}
```