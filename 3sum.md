## 3Sum

### Problem

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:** The solution set must not contain duplicate triplets.

```
For example, given array S = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

**Related Topics:**

`Array` `Two Pointers`

### Analysis

先确定第一个值 `a1`，则 `a2 + a3 = - a1`，只需对两个数进行查找即可。之后可以考虑将两个数，分别从两端向中间遍历，减少一重循环。但是这样做的前提是，数组必须有序。只有数组有序，在 `a2 + a3 != - a1` 时，才能确定左右两端，哪一端向中间靠近。

当 `a2` 与 `a3` 确定时，避免出现重复，可以排除掉 `a2` 与 `a3` 相邻的相同数。

### Code

```kotlin
class Solution {

    fun threeSum(nums: IntArray): List<List<Int>> {

        nums.sort()

        var result: MutableList<List<Int>> = mutableListOf()
        var left: Int
        var right: Int
        var sum: Int

        for (i in 0 until nums.size - 2) {
            if (i == 0 || nums[i] != nums[i - 1]) {

                left = i + 1
                right = nums.size - 1
                sum = 0 - nums[i]

                while (left < right) {
                    when {
                        nums[left] + nums[right] == sum -> {
                            result.add(arrayListOf(nums[i], nums[left], nums[right]))
                            while (left < right && nums[left] == nums[++left]) {
                            }
                            while (left < right && nums[right] == nums[--right]) {
                            }
                        }
                        nums[left] + nums[right] < sum -> left++
                        else -> right--
                    }
                }
            }
        }

        return result
    }
}
```