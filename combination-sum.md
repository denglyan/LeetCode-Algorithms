## Combination Sum

### Problem

Given a set of candidate numbers (`candidates`) (without duplicates) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The same repeated number may be chosen from `candidates` unlimited number of times.

**Note:**
- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

**Related Topics:**

`Array` `Backtracking`

### Analysis

深度优先搜索，回溯。

### Code

```kotlin
class Solution {

    fun combinationSum(candidates: IntArray, target: Int): List<List<Int>> {

        candidates.sort()

        val result = mutableListOf<List<Int>>()
        backtrack(candidates, result, intArrayOf(), target, 0)

        return result
    }

    private fun backtrack(candidates: IntArray, result: MutableList<List<Int>>, tempArray: IntArray, target: Int, index: Int) {

        when {
            target == 0 -> {
                result.add(tempArray.toList())
            }
            target > 0 -> {
                (index until candidates.size).forEach {
                    backtrack(candidates, result, tempArray + candidates[it], target - candidates[it], it)
                }
            }
        }
    }
}
```