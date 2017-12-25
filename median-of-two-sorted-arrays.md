## Median of Two Sorted Arrays

### Problem

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

**Example 1:**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

**Related Topics:**

`Array` `Binary Search` `Divide and Conquer`

### Analysis

中位数：将一个集合分成等长的两部分，其中一部分总是小于另一部分。

我们可以将 `A`，`B` 两个数组进行划分：

```
      left_part          |        right_part
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```

如果我们确保：

```
1) len(left_part) == len(right_part)
2) max(left_part) <= min(right_part)

等价于

(1) i + j == m - i + n - j (or: m - i + n - j + 1)
    if n >= m, we just need to set: i = 0 ~ m, j = (m + n + 1)/2 - i
(2) B[j-1] <= A[i] and A[i-1] <= B[j]

n >= m，为了确保 j 非负
```

则可得到结果：

```
median = (max(left_part) + min(right_part))/2

等价于

max(A[i-1], B[j-1]) (when m + n is odd)
or (max(A[i-1], B[j-1]) + min(A[i], B[j]))/2 (when m + n is even)
```

所以我们需要做的是：

```
Searching i in [0, m], to find an object `i` that:
    B[j-1] <= A[i] and A[i-1] <= B[j], ( where j = (m + n + 1)/2 - i )
```

通过二分查找，确定 `i`：

```
<1> Set imin = 0, imax = m, then start searching in [imin, imax]

<2> Set i = (imin + imax)/2, j = (m + n + 1)/2 - i

<3> Now we have len(left_part)==len(right_part). And there are only 3 situations
     that we may encounter:
    <a> B[j-1] <= A[i] and A[i-1] <= B[j]
        Means we have found the object `i`, so stop searching.
    <b> B[j-1] > A[i]
        Means A[i] is too small. We must `ajust` i to get `B[j-1] <= A[i]`.
        Can we `increase` i?
            Yes. Because when i is increased, j will be decreased.
            So B[j-1] is decreased and A[i] is increased, and `B[j-1] <= A[i]` may
            be satisfied.
        Can we `decrease` i?
            `No!` Because when i is decreased, j will be increased.
            So B[j-1] is increased and A[i] is decreased, and B[j-1] <= A[i] will
            be never satisfied.
        So we must `increase` i. That is, we must ajust the searching range to
        [i+1, imax]. So, set imin = i+1, and goto <2>.
    <c> A[i-1] > B[j]
        Means A[i-1] is too big. And we must `decrease` i to get `A[i-1]<=B[j]`.
        That is, we must ajust the searching range to [imin, i-1].
        So, set imax = i-1, and goto <2>.
```

加上边界值：

```
<a> (j == 0 or i == m or B[j-1] <= A[i]) and
    (i == 0 or j = n or A[i-1] <= B[j])
    Means i is perfect, we can stop searching.

<b> i < m and B[j - 1] > A[i]
    Means i is too small, we must increase it.

<c> i > 0 and A[i - 1] > B[j]
    Means i is too big, we must decrease it.
```

`<b>` 和 `<c>` 不需要判断 `j`，因为 `i < m ==> j > 0` 和 `i > 0 ==> j < n`：

```
m <= n, i < m ==> j = (m+n+1)/2 - i > (m+n+1)/2 - m >= (2*m+1)/2 - m >= 0
m <= n, i > 0 ==> j = (m+n+1)/2 - i < (m+n+1)/2 <= (2*n+1)/2 <= n
```

### Code

```kotlin
class Solution {

    fun findMedianSortedArrays(nums1: IntArray, nums2: IntArray): Double {

        val m: Int
        val n: Int
        val a: IntArray
        val b: IntArray
        if (nums1.size <= nums2.size) {
            a = nums1
            b = nums2
        } else {
            a = nums2
            b = nums1
        }
        m = a.size
        n = b.size

        var i: Int
        var j: Int
        val halfLen = (m + n + 1) / 2
        var minI = 0
        var maxI = m
        while (minI <= maxI) {

            i = (minI + maxI) / 2
            j = halfLen - i

            when {
                i < m && b[j - 1] > a[i] -> minI = i + 1
                i > 0 && a[i - 1] > b[j] -> maxI = i - 1
                else -> {
                    val maxLeft: Int = when {
                        i == 0 -> b[j - 1]
                        j == 0 -> a[i - 1]
                        else -> maxOf(a[i - 1], b[j - 1])
                    }

                    if ((m + n) % 2 == 1) {
                        return maxLeft * 1.0
                    }

                    val minRight: Int = when {
                        i == m -> b[j]
                        j == n -> a[i]
                        else -> minOf(a[i], b[j])
                    }

                    return (maxLeft + minRight) / 2.0
                }
            }
        }

        throw Exception("ValueError")
    }
}
```