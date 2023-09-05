---
title: Merge Sort
date: 2023-09-03T08:00:41+08:00
weight: 1
tags:
- Algorithm
- sorts
---

{{< toc >}}


## Basic plan

- Divide array into two halves.
- Recursively sort each half.
- Merge two halves



## Java implementation

```
    private static void mergeSort(int [] a) {
        int [] aux = new int[a.length];
        mergeSortHelper(a, aux, 0, a.length -1);
    }

    private static void mergeSortHelper(int [] a, int [] aux, int lo, int hi) {
        if (hi <= lo) {
            return;
        }
        int mid = lo + (hi - lo) / 2;
        mergeSortHelper(a, aux, lo, mid);
        mergeSortHelper(a, aux, mid + 1, hi);
        merge(a, aux, lo, mid, hi);

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

