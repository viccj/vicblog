---
title: "238. Product of Array Except Self "
date: 2023-08-09T07:25:41+08:00
weight: 7
tags:
- leetcode
- java
- array
---
Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.



**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

**Example 2:**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```



## 解題思路

### 個人解

一開始想到，那我不就全部先乘起來，遇到每個值之後用成起來的結果除以自身就可以得到答案，但太天真了，忘了自己是 0 的狀況。



後來處理了自己是 0 的狀況之後，因為預設乘積是 1，處理不到全部都是 0 的狀況。



全部是0 的狀況處理完之後，又沒處理到 0 的個數 > 2的狀況



最後處理完之後發現，基本上超過2個0，回傳的答案就會全部是 0



經過一堆 fail submit之後 總算 pass 了



```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int product = 1;
        int zeroCount = 0;
        int[] result = new int[nums.length];
        
        // Calculate the product of all elements except zeros.
        for (int num : nums) {
            if (num != 0) {
                product *= num;
            } else {
                zeroCount++;
            }
        }

        // Fill the result array.
        for (int i = 0; i < nums.length; i++) {
            if (zeroCount > 1) {
                result[i] = 0;
            } else if (zeroCount == 1) {
                result[i] = nums[i] == 0 ? product : 0;
            } else {
                result[i] = product / nums[i];
            }
        }

        return result;
    }
}

```



## 其他解



網路通常都是用這種解法，比較單純，但我一開始想不到，第二次回來做也還是忘了...

是先把這個數左邊的乘積記錄下來，再把右邊的乘積記錄下來，產生兩個 Array (要一個也可以)

最後在把這兩個 Array 對應的 index 乘起來就是答案。



```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] prefixProducts = new int[n];
        int[] suffixProducts = new int[n];
        int[] result = new int[n];
        
        // Calculate prefix products
        int prefix = 1;
        for (int i = 0; i < n; i++) {
            prefixProducts[i] = prefix;
            prefix *= nums[i];
        }
        
        // Calculate suffix products
        int suffix = 1;
        for (int i = n - 1; i >= 0; i--) {
            suffixProducts[i] = suffix;
            suffix *= nums[i];
        }
        
        // Calculate the result array by combining prefix and suffix products
        for (int i = 0; i < n; i++) {
            result[i] = prefixProducts[i] * suffixProducts[i];
        }
        
        return result;
    }
}

```



