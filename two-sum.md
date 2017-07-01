## Two Sum

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

### 描述

给出一组整数，以及一个目标，假设必存在两个数相加的结果等于这个目标，输出这两个数在数组里的下标

### 分析

- 方法一：暴力查找，通过双重循环进行查找
- 方法二：采用 HashMap 来存储数据，以空间换时间，优化查找
- 方法三：在方法二的基础上，将两次查询合并成一次

### 代码

**暴力查找**

```java
public class Solution {

    public int[] twoSum(int[] nums, int target) {

        for (int i = 0, k; i < nums.length; i++) {

            k = target - nums[i];
            for (int j = i + 1; j < nums.length; j++) {

                if (nums[j] == k) {
                    return new int[]{i, j};
                }
            }
        }

        throw new IllegalArgumentException("No two sum solution");
    }
}
```

**HashMap 存储数据**

```java
import java.util.HashMap;

public class Solution {

    public int[] twoSum(int[] nums, int target) {

        HashMap<Integer, Integer> hashMap = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++) {
            hashMap.put(nums[i], i);
        }

        for (int i = 0, k; i < nums.length; i++) {

            k = target - nums[i];
            if (hashMap.containsKey(k) && hashMap.get(k) != i) {
                return new int[]{i, hashMap.get(k)};
            }
        }

        throw new IllegalArgumentException("No two sum solution");
    }
}
```

**优化**

```java
import java.util.HashMap;

public class Solution {

    public int[] twoSum(int[] nums, int target) {

        HashMap<Integer, Integer> hashMap = new HashMap<Integer, Integer>();
        for (int i = 0, k; i < nums.length; i++) {

            k = target - nums[i];
            if (hashMap.containsKey(k)) {
                return new int[]{hashMap.get(k), i};
            }

            hashMap.put(nums[i], i);
        }

        throw new IllegalArgumentException("No two sum solution");
    }
}
```