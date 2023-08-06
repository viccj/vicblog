---
title: "1. Two Sum"
date: 2023-08-03T13:00:41+08:00
weight: 1
tags:
- leetcode
- array
- java
---

# Two Sum

## 暴力解

兩個 loop 加起來等於 target, 就回傳他們的 index，應該是最新手最直覺的人（我）都想得出來的解答

## 優化解

一樣是用兩個 loop 去解，但是這裡不是用 nested loop，而是先用一個 loop 以及 hashmap 把陣列裡的每個值標記起來 [index, value]，另一個 loop 再去對每個值做計算，如果 target - nums[i] 有在 hashmap裏面，就回傳 i 以及對應的 index



```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> hashmap = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {
            hashmap.put(nums[i], i);
        }

        for (int i = 0; i < nums.length; i++) {
            int difference = target - nums[i];
            if(hashmap.containsKey(difference) && hashmap.get(difference) != i) {
                return new int[] {i, hashmap.get(difference)};
            }
        }
        return new int[]{};
    }
}

```

