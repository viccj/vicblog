---
title: "121. Best Time to Buy and Sell Stock"
date: 2023-06-18T22:00:41+08:00
weight: 2
tags:
- leetcode
- java
- array
---

## 解題思路

要如何算出最大收益？ 買的價格越低，賣的價格越高。

但因為買的時間一定要比賣的時間早，所以只能由左至右開始搜尋（index 從 0 開始）

一開始也只能知道一開始的收益以及目前的金額



假如給定的價格列表為`[7,1,5,3,6,4]`

一開始的收益為 `profit = 0`

一開始的金額是 `prices [0]`



如果是第一天買股票，第二天賣

這時候的收益會是 1 - 7 = - 6

再往後計算的話

如果是第一天買，第三天賣

這時候的收益會是 5 - 7 = - 2

看起來比第一天買第二天賣還要賺（虧比較少）

---

但這時發現了一件事，第二天的價格比第一天還要低，
這樣根本不用再去管第一天的價格是多少，因為用第二天的價格 (1) 當買入價來繼續計算，
算出來的收益一定會比用第一天的價格 (7) 當買入價還要划算。
而且時間不能重來。


因此可以在計算價差時，發現最低價的時候就更新最低價的值，
計算價差的時候就只要用後續的價格 - 最低價即可得到最大收益。


依照範例給的數據會得到以下結果：
```agsl
Profit =  price[index] - min

1 - 7 = -6

Min = 1

1 - 1 = 0, min = 1

5 - 1 = 4, min = 1, profit = 0

3 - 1 = 2, min = 1, profit = 2

6 - 1 = 5, min = 1, profit = 6

6 - 1 = 6, min = 1, profit = 5



Profit = 5, at 1 and 6

```

## 程式碼
```java
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        int min = prices[0];
        int profit = 0;
        
        for (int i = 0; i < len; i++) {
            if (prices[i] < min) {
                min = prices[i];
            }
            if (prices[i] - min > profit) {
                profit = prices[i] - min;
            }
        }
        return profit;
    }
}
```