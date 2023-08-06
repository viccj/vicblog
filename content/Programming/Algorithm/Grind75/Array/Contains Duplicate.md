---
title: "217. Contains Duplicate"
date: 2023-08-06T09:00:41+08:00
weight: 4
tags:
- leetcode
- java
- array
---
Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.



**Example 1:**

```
Input: nums = [1,2,3,1]
Output: true
```

**Example 2:**

```
Input: nums = [1,2,3,4]
Output: false
```

**Example 3:**

```
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```



## 解題思路



一樣用hash map 統計有無重複出現的



```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashMap<Integer, Integer> hashmap = new HashMap<>();
        int len = nums.length;

        for(int i = 0; i < len; i ++) {
            hashmap.put(nums[i], hashmap.getOrDefault(nums[i], 0) + 1);
            if (hashmap.get(nums[i]) > 1) {
                return true;
            }
        }
 
        return false;
    }
}
```

