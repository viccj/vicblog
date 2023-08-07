---
title: "57. Insert Interval"
date: 2023-08-07T08:00:41+08:00
weight: 4
tags:
- leetcode
- java
- array
---
You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` *after the insertion*.



**Example 1:**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```



## 解題思路



1. 建立一個新的`ArrayList ans`

2. 把這個題目用數線來思考，從左邊開始檢查。對 `newInterval[0]` 以及 `intervals[[i][0]]` 比較，若是 `newInterval[0]` 較大，就把`intervals[i]` 加到 `ans` 內，因為代表 `newInterval` 的起始點會再加入的這些數線的起始點之後。

3. 處理 edge case:

    - 如果巡過一輪，發現 `ans`是空的，代表新的數線最小值是最小的。直接把數線加進去

    - 新的數線的最小值比所有數線的最小值大，這時候直接把 `newInterval` 加進去。ex `intervals = [ [1,3], [4 ,5] ], newInterval = [6, 7], ans = [[1,3], [4 , 5], [6,7]]`

4. 處理最大值：加完之後將當下`ans`的最大值與最新數線的最大值比較後取最大的

5. 比較剩下的數線，若剩下的束線最小值小於目前數線的最小值，檢查兩者的最大值後取最大的

6. 將剩下的數線加入`ans`內

7. ans ArrayList轉換為2D陣列



```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        ArrayList<int[]> ans = new ArrayList<>();
        int len = intervals.length;
        int i = 0;
        while (i < len && intervals[i][0] <newInterval[0]) {
            ans.add(intervals[i]);
            i++;
        }
        //edge case
        if (ans.size() == 0 || newInterval[0] > ans.get(ans.size() - 1)[1]) {
            ans.add(newInterval);
        } else {
            int[] lastInterval = ans.get(ans.size() - 1);
            lastInterval[1] = Math.max(lastInterval[1], newInterval[1]);
        }
        // compare right side of line(interval
        while (i < len) {
            int[] lastInterval = ans.get(ans.size() - 1);
            if (lastInterval[1] >= intervals[i][0]) {
                lastInterval[1] = Math.max(lastInterval[1], intervals[i][1]);
            } else{
                // If the last interval does not overlap with the current interval, add it to the answer list
                ans.add(intervals[i]);
            }
            i++;
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```

