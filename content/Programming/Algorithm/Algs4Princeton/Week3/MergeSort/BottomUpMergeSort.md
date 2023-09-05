---
title: Bottom-up Merge Sort
date: 2023-09-05T08:00:41+08:00
weight: 2
tags:
- Algorithm
- sorts
---

{{< toc >}}


## Basic plan

- Pass through array, merging subarrays of size 1.
- Repeat for subarrays of size 2, 4, 8, .. until end of array.
- no recursion needed


## Java implementation

```
    private static void mergeSort(int [] a) {
        int [] aux = new int[a.length];
        for (int sz = 1; sz < a.lentgh, sz = sz + sz) {
            for (int lo = 0; lo < N-sz; lo += sz+sz) {
                merge(a, aux, lo, lo+sz-1, Math.min(lo+sz+sz-1, N-1)
            }
        }
    }

    private static void merge(int [] a, int [] aux, int lo, int mid, int hi) {

        // copy
        for (int i :a) {
            aux[i] = a[i];
        }

        int i = lo, j = mid + 1;

        for (int k = lo; k <= hi; k++) {

            // 如果 i 已經 超過 mid, 代表i 左半邊子樹列已經全部遍歷過一次，
            // 那麼接下來a[k] 就直接會是右半邊的下一個 item aux[j]
            if (i > mid) {
                a[k] = aux[j];
                j++;

            // 如果 j 已經 超過 hi, 代表右半邊子樹列已經全部遍歷過一次，
            // 那麼接下來a[k] 就直接會是左半邊的下一個 item aux[i]
            } else if (j > hi) {
                a[k] = aux[i];
                i++;

            // 比較小的放進a[k]內
            } else if(less(aux[j], aux[i])) {
                a[k] = aux[j];
                j++;
            } else {
                a[k] = aux[i];
                i++;
            }
        }

    }
```




## Reference

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/22Mergesort.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}

