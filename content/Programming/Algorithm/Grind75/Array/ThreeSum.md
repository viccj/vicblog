---
title: "15. Three Sum"
date: 2023-08-08T08:25:41+08:00
weight: 6
tags:
- leetcode
- java
- array
---
Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.



**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```

**Example 2:**

```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

**Example 3:**

```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```





## 解題思路



> 如果有做過Two Sum跟 Two Sum 2的話，這一題是非常直覺的

但我沒做過Two Sum 2，也不覺得直覺，更知道一定不可能是3個for loop。直到看到

> A + B + C = 0

就等於是Two Sum 的題目，`target = -C`, 找出`A,B`

所以一樣可以用HashMap來做這一題，但是這次學了不同的方法，用 Left, Right point來實作



```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        
        List<List<Integer>> res = new ArrayList<>();
        
        // 因為要用左右指針，所以先將原來的Array排序
        Arrays.sort(nums);

        for (int i = 0; i < nums.length; i++) {
            int a = nums[i];
            
            // 因為不能出現重複的Array，如果 a 右邊的下一個數字與a相同，直接跳過
            if (i > 0 && a == nums[i - 1]) {
                continue;
            }

            // 定義左右指針
            int l = i + 1;
            int r = nums.length - 1;
            
            // 在左指針超過右指針前，瘋狂迴圈
            while (l < r) {
                int threeSum = nums[l] + nums[r] + a;

                    // 如果加起來的結果大於0，代表右邊的數字太大了，將右指針 -1
                if (threeSum > 0) {
                    r --;
                    
                    // 如果加起來的結果小於0，代表左邊的數字太小了，將左指針 +1
                } else if (threeSum < 0) {
                    l ++;
                } else {
                    // 等於0，找到一組，加進ArrayList內
                    res.add(Arrays.asList(a, nums[l], nums[r]));
                    
                    // 左指針加一，重新開始找下一個組合
                    // 但為了減少不必要的查找，如果左指針 +1 後跟上次一樣，就再 +1
                    l++;
                    while (nums[l] == nums[l - 1] && l < r) {
                        l++;
                    }
                    
                    // 右指針則是相反（但其實可以忽略右指針，只換左指針也可以）
                    r--;
                    while (nums[r] == nums[r + 1] && l < r) {
                        r--;
                    }
                    
                }
            }
        }
        return res;
    }
}
```



