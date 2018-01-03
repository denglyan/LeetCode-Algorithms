## Container with Most Water

### Problem

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

**Related Topics:**

`Array` `Two Pointers`

### Analysis

![](/assets/container-with-most-water.png)

一个容器所能装的水量为： `length * min(height[l] , height[r])`。

`l` 与 `r` 分别从两端开始，向中间遍历。当 `height[l] < height[r]` 时，最小边为 `l`，移动 `r` 并不能扩大面积，所以移动 `l`。反之，移动 `r` 即可。

### Code

```kotlin
class Solution {

    fun maxArea(height: IntArray): Int {

        var max = 0
        var l = 0
        var r = height.size - 1

        while (l < r) {
            max = maxOf(max, (r - l) * if (height[l] < height[r]) height[l++] else height[r--])
        }

        return max
    }
}
```