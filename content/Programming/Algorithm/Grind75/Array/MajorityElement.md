---
title: "169. Majority Element"
date: 2023-06-18T22:00:41+08:00
weight: 3
tags:
- leetcode
- java
- array
---

Given an array `nums` of size `n`, return *the majority element*.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.



**Example 1:**

```
Input: nums = [3,2,3]
Output: 3
```

**Example 2:**

```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```



## 思路:

用 hashmap 統計每個直出現的次數，最後再找出最大的值

```java
class Solution {
    public int majorityElement(int[] nums) {
        HashMap<Integer, Integer> hashmap = new HashMap<>();
        int len = nums.length;

        for(int i = 0; i < len; i ++) {
            if(!hashmap.containsKey(nums[i])) {
                hashmap.put(nums[i], 1);
            } else {
                hashmap.put(nums[i], hashmap.get(nums[i]) + 1);
            }
        }
         int halfLen = len / 2;
         for (Map.Entry<Integer, Integer> entry : hashmap.entrySet()) {
             if (entry.getValue() > halfLen) {
                 return entry.getKey();
             }
         }
        return -1;

    }
}
```
## 改寫

在記錄的時候，連帶檢查每個 element出現的次數，只要出現的次數大於 Array 的長度，就可以確定他是這個 Array 的 majority


```java
class Solution {
    public int majorityElement(int[] nums) {
        HashMap<Integer, Integer> hashmap = new HashMap<>();
        int len = nums.length;

        for(int i = 0; i < len; i ++) {
            if(!hashmap.containsKey(nums[i])) {
                hashmap.put(nums[i], 1);
            } else {
                hashmap.put(nums[i], hashmap.get(nums[i]) + 1);
            }

            if (hashmap.get(nums[i]) > len / 2) {
                return nums[i];
            }
        }
   
        return -1;

    }
}
```