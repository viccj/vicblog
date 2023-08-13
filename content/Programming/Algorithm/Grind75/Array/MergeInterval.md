---
title: "56. Merge Interval"
date: 2023-08-13T16:25:41+08:00
weight: 8
tags:
- leetcode
- java
- array
---
Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the

frequency

of at least one of the chosen numbers is different.



The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.



**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```



Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.



**Example 1:**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

**Example 2:**

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```



## 解題思路

跟之前有做過的57. Insert Interval一樣，但這題比較簡單。

一樣是用數線的概念來解，若下一個數線的頭 < 上一個數線的尾巴時，上一個數線的尾巴會等於兩段數線尾巴的最大值。

ps. 這題要先sort, 所以 （Time Complexity）：O(n log n) + O(n) = O(n log n)

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        List<int[]> merged = new ArrayList<>();

        for (int i = 0; i < intervals.length; i++) {
            int[] currentInterval = intervals[i];

            if(merged.isEmpty() || currentInterval[0] > merged.get(merged.size() -1)[1]) {
                merged.add(currentInterval);
            } else {
                merged.get(merged.size() - 1)[1] = Math.max(currentInterval[1], merged.get(merged.size() - 1)[1]);
            }
        }
        return merged.toArray(new int[merged.size()][]);
    }
}
```


