---
title: Insertion Sort
date: 2023-08-26T08:00:41+08:00
weight: 2
tags:
- Algorithm
- sorts
---

{{< toc >}}



## Algorithm

In iteration `i`, swap `a[i]` with each larger entry to its left



## Insertion sort inner loop

To maintain algorithm invariants:

- move the pointer to the right

  `i++`

- Moving from right to left, exchange `a[i]` with each larger entry to its left.


## demo


![quick-find-example](https://raw.githubusercontent.com/viccj/upic/master/uPic/insertion-sort.png)




## Inner for loop

```
  for (int j = i; j > 0 ; j--) {
          if(less(a[j], a[j-1])) {
              exch(a, j, j-1);
          } else {
              break;
          }
      }
```



## Java implementation

```
public static void insertionSort(int[] a) {
    int N = a.length;
    for (int i = 1; i < N; i++) {
        for (int j = i; j > 0 ; j--) {
            if(less(a[j], a[j-1])) {
                exch(a, j, j-1);
            } else {
                break;
            }
        }
    }
}
```

## Analysis

- Best case. 如果 array 已經排列好，insertion sort 只比較了 N-1 次，且無任何交換

- Worst case. 如果 array 是 descending order (且無重複)，那麼比了 1/2N^2次也交換了 1/2N^2 次

best: O(N)
worst: O(N^2)



## Reference

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/21ElementarySorts.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}

