## Two Sum

### Problem

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the same element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

**Related Topics:**

`Array` `Hash Table`

### Analysis

- 方法一：暴力查找，双重循环判断。

- 方法二：用 `Map` 来存储数据，以值为 `key`，以下标为 `value`。先判断 `key` 是否等于 `target - nums[i]`，再往 `Map` 里存储数据，就可以避免出现 `value == i` 的情况。

### Code

**暴力查找**

```kotlin
class Solution {

    fun twoSum(nums: IntArray, target: Int): IntArray {

        for (i: Int in nums.indices) {

            val k = target - nums[i]
            for (j: Int in IntRange(i + 1, nums.lastIndex)) {
                if (k == nums[j]) {
                    return intArrayOf(i, j)
                }
            }
        }

        throw IllegalArgumentException("No two sum solution")
    }
}
```

**Map 存储数据**

```kotlin
class Solution {

    fun twoSum(nums: IntArray, target: Int): IntArray {

        val map = HashMap<Int, Int>()

        nums.forEachIndexed { i, n ->

            val index = map[target - n]
            if (index != null) {
                return intArrayOf(i, index)
            }

            map[n] = i
        }

        throw IllegalArgumentException("No two sum solution")
    }
}
```