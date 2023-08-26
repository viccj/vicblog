---
title: Shell Sort
date: 2023-08-26T08:00:41+08:00
weight: 3
tags:
- Algorithm
- sorts
---

{{< toc >}}


## Idea

基本上就是做了很多次 insertion sort，但 insertion sort 是往左邊一個比較，比較的間隔為1，Shell sort 每次會比較 h 個間隔，隨著 h 的遞減，直到h <1為止。



也就是說 insertion sort 就是 h = 1 的 shell sort.



```Java
public class Shell
{
  public static void sort(Comparable[] a) {
    int N = a.length;
    
    // 得出初始的h值，這邊是用3N+1，也可以嘗試用不同的算式
    int h = 1;
    while (h < N/3) {
      h = 3*h + 1;
    }
    
    
    while (h >= 1) {
      
      // 不同的 h 去做的insert sort， 之前的insertion sort的話其實h就是1
      for (int i = h; i < N; i++) {
        for (int j = i; j >= h && less(a[j], a[j-h]); j -=h) {
          exch(a, j, j-h);
        }
      }
      // 遞減 h 直到 h < 1為止
      h = h/3;
    }
  }
}
```



## Reference

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/21ElementarySorts.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}

