## Next Permutation

### Problem

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
`1,2,3 → 1,3,2`
`3,2,1 → 1,2,3`
`1,1,5 → 1,5,1`

**Related Topics:**

`Array`

### Analysis

首先明确：
- 当序列是降序时（最大序列），不存在比这个序列大的序列
- 当序列是升序时（最小序列），不存在比这个序列小的序列

从后往前遍历数组，找到第一个破坏降序的数，将其与降序中，仅比自身大的数，进行交换（此时序列已大于原序列），然后将降序变为升序即可。

### Code

```kotlin
class Solution {

    fun nextPermutation(nums: IntArray): Unit {

        val leftIndex = (nums.lastIndex - 1 downTo 0).firstOrNull { nums[it + 1] > nums[it] } ?: -1
        val rightIndex: Int

        if (leftIndex >= 0) {
            rightIndex = (leftIndex until nums.lastIndex).firstOrNull { nums[leftIndex] >= nums[it + 1] } ?: nums.lastIndex
            nums.swap(leftIndex, rightIndex)
        }

        nums.reverse(leftIndex + 1)
    }

    private fun IntArray.reverse(index: Int) {

        var start = index
        var end = this.lastIndex

        while (start < end) {
            this.swap(start++, end--)
        }
    }

    private fun IntArray.swap(left: Int, right: Int) {

        val temp = this[right]
        this[right] = this[left]
        this[left] = temp
    }
}
```