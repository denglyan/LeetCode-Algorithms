## Median of Two Sorted Arrays

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

`Binary` `Search Array` `Divide and Conquer`

### 描述

给出两个已经有序的数组，求两个数组合并后的中位数，且时间复杂度要求为 `O(log (m+n))`

### 分析

如果采用归并排序的方式合并数组，再求中位数，其时间复杂度为 `O(m+n)`，不符合要求。从这个时间复杂度中，可以看出，主要采用二分搜索来查找中位数

讨论区里有一个很不错的分析，这里简单介绍一下，建议大家可以看看原文：[solution](https://discuss.leetcode.com/topic/4996/share-my-o-log-min-m-n-solution-with-explanation)

首先，对于中位数，要有一个明确的认识：把一个集合分成等长的两部分，其中一部分所有值总是小于另一部分

```
若：len(left_part) == len(right_part) && max(left_part) <= min(right_part)
则：median = (max(left_part) + min(right_part))/2


      left_part          |        right_part
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]


所以目标是：
Searching i in [0, m], to find an object `i` that:
    B[j-1] <= A[i] and A[i-1] <= B[j], ( where j = (m + n + 1)/2 - i )


当 B[j-1] > A[i] 时，说明 A[i] 偏小，Searching i in [i+1, m]
当 A[i-1] > B[j] 时，说明 A[i-1] 偏大，Searching i in [0, i-1]


边界值：
若 i = 0，说明 A[0] ... A[i-1] 不存在，则 max(left_part) 就是 B[j-1]
若 j = 0，说明 B[0] ... B[j-1] 不存在，则 max(left_part) 就是 A[i-1]
若 i = m，说明 A[i] ... A[m-1] 不存在，则 min(right_part) 就是 B[j]
若 j = n，说明 B[j] ... B[n-1] 不存在，则 min(right_part) 就是 A[i]
```

### 代码

```java
public class Solution {

    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        if (nums1.length > nums2.length) {
            int[] tmp = nums1;
            nums1 = nums2;
            nums2 = tmp;
        }

        int i, j;
        int m = nums1.length;
        int n = nums2.length;

        int imin = 0;
        int imax = m;
        int half_len = (m + n + 1) / 2;

        while (imin <= imax) {

            i = (imin + imax) / 2;
            j = half_len - i;

            if (i < m && nums1[i] < nums2[j - 1]) {
                imin = i + 1;
            } else if (i > 0 && nums1[i - 1] > nums2[j]) {
                imax = i - 1;
            } else {

                int max_of_left;
                if (i == 0) {
                    max_of_left = nums2[j - 1];
                } else if (j == 0) {
                    max_of_left = nums1[i - 1];
                } else {
                    max_of_left = Math.max(nums1[i - 1], nums2[j - 1]);
                }

                if ((m + n) % 2 == 1) {
                    return max_of_left;
                }

                int min_of_right;
                if (i == m) {
                    min_of_right = nums2[j];
                } else if (j == n) {
                    min_of_right = nums1[i];
                } else {
                    min_of_right = Math.min(nums1[i], nums2[j]);
                }

                return (max_of_left + min_of_right) / 2.0;
            }
        }

        return 0;
    }
}
```