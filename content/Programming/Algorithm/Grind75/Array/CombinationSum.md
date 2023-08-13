---
title: "39. Combination Sum "
date: 2023-08-13T10:25:41+08:00
weight: 8
tags:
- leetcode
- java
- array
- recursive
- backtrack
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





## 知識點

- 如果題目要求我們去**排列 (Permutations)** 或**組合 (Combinations)** 的時候，通常可以用 **Back Tracking** 的方法來處理。
- Back Tracking 的基本架構是：
    - **Base Case（基本情況）：** 在遞迴函式開始處，檢查是否已經達到了終止條件，這被稱為基本情況。如果終止條件成立，那麼這是遞迴的終止點，你可以在這裡處理結果並返回。這是遞迴的基礎。
    - **Iterate Input（遍歷輸入）：** 遍歷題目給出的輸入，這通常是一個列表、陣列或其他類型的集合。在每一個迴圈中，選擇其中的一個元素，然後對其進行相應的操作。這個迴圈允許你嘗試所有可能的選擇。
    - **Call Recursive（遞迴呼叫）：** 在迴圈中，對於每個元素，你都會進行遞迴呼叫。這就是“深入”問題的一部分。將更新後的狀態（可能是已經選擇的元素、新的索引等）傳遞給遞迴函式。在不同的狀態下進一步處理問題。
    - **Restore/Update State（回復/更新狀態）：** 在每個遞迴返回後，你都需要確保狀態的一致性。如果在遞迴過程中進行了狀態的修改，則需要恢復到遞迴前的狀態（回復狀態）。如果在遞迴過程中加入了新的選擇，則需要將其刪除，以確保下一輪迴圈可以嘗試其他選擇（更新狀態）。



```python
def backtrack(result, state, choices, parameters):
    if termination_condition_is_met:
        process_result
        return
    
    for choice in choices:
        if choice meets conditions:
            update_state
            backtrack(result, new_state, choices, new_parameters)
            restore_state
```



## 本題情況

- **Base Case：** 用減法的話 `target - list 的 sum =0 || < 0` 都是 base case
- **Iterate Input / Call Recursive：** 建立一個 `tmpList` 將元素一個一個加進去後檢查 target 剩下多少
- **Restore/Update State：** 如果遇上 base case的話，return 掉，並將 tmpList 最後一位pop掉



```
Recursive(input, [], 7, 0);

Recursive(input, [2], 7-5, 0);

Recursive(input, [2, 2], 5-2, 0);

Recursive(input, [2, 2, 2], 3-2, 0);

Recursive(input, [2, 2, 2, 2], 1-2, 0); //return and pop last item in tmpList, enter in next for loop

Recursive(input, [2, 2, 2, 3], 1-3, 1); //return and pop last item in tmpList, enter in next for loop

Recursive(input, [2, 2, 2, 6], 1-5, 2); //return and pop last item in tmpList, enter in next for loop

Recursive(input, [2, 2, 2, 7], 1-7, 3); //return and pop last item in tmpList, enter in next for loop

// end for loop 

// pop last item in tmpList

Recursive(input, [2, 2, 2], 3-2, 0);  pop last item in tmpList, enter in next for loop

Recursive(input, [2, 2, 3], 3-3, 1);  pop last item in tmpList, enter in next for loop, add to result

Recursive(input, [2, 2, 6], 3-6, 2);  pop last item in tmpList, enter in next for loop

Recursive(input, [2, 2, 7], 3-7, 2);  pop last item in tmpList, enter in next for loop


// end for loop 

// pop last item in tmpList

Recursive(input, [2, 2], 5-2, 0); pop last item in tmpList, enter in next for loop

Recursive(input, [2, 3], 5-3, 1)

...

```
## Java Implement

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), candidates, target, 0);
        return result;
    }

       private void backtrack(List<List<Integer>> result, List<Integer> tmpList, int[] candidates, int remain, int index){
        if (remain < 0) {
            return;
        } 
        if (remain == 0) {
            result.add(new ArrayList<>(tmpList));
            return;
        }

        for (int i = index; i < candidates.length; i++) {
            int newRemain = remain - candidates[i];
            tmpList.add(candidates[i]);
            backtrack(result, tmpList, candidates, newRemain, i);
            tmpList.remove(tmpList.size() - 1);
        }
    }
}
```







