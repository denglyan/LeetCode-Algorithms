## 3Sum Closest

### Problem

Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

```
    For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**Related Topics:**

`Array` `Two Pointers`

### Analysis

核心思想与 `3Sum` 相同。

这里需要注意：由于这题并不像 `3Sum` 能在过程中确定 `a2` 与 `a3`，所以不能像 `3Sum` 的解法一样跳过相邻的相同数。

### Code

```kotlin
class Solution {

    fun threeSumClosest(nums: IntArray, target: Int): Int {

        nums.sort()

        var left: Int
        var right: Int
        var sub: Int
        var min = Int.MAX_VALUE

        loop@ for (i in 0 until nums.size - 2) {
            if (i == 0 || nums[i] != nums[i - 1]) {

                left = i + 1
                right = nums.size - 1

                while (left < right) {

                    sub = nums[left] + nums[right] + nums[i] - target
                    if (Math.abs(sub) < Math.abs(min)) {
                        min = sub
                    }

                    when {
                        sub == 0 -> break@loop
                        sub < 0 -> left++
                        else -> right--
                    }
                }
            }
        }

        return min + target
    }
}
```