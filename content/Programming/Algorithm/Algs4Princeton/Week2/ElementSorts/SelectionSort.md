---
title: Selection Sort
date: 2023-08-22T08:00:41+08:00
weight: 1
tags:
- Algorithm
- sorts
---

{{< toc >}}

- In iteration i, find index min of smallest reamining entry.
- Swap `a[i]` and `a[min]`


![quick-find-example](https://raw.githubusercontent.com/viccj/upic/master/uPic/selection-sort.png)


## Template for sort classes

```java
public class Example
{
  public static void sort(Comparable[] a) {
    /* sort algorithms need to implement */
  }
  private static boolean less(Comparable v, Comparable w) {
    return v.compareTo(w) < 0;
  }
  private int compareTo(Comparable v, Comparable w) {
    if(v > w) {
      return 1;
    }
    if(v < w) {
      return -1;
    }
    return 0;
  }
  private static void exch(Comparable[] a, int i, int j) {
    Comparable swap = a[i];
    a[i] = j;
    a[j] = swap;
  }
  public static boolean isSorted(Comparable[] a) {
    // Test whether the array entries are in order.
    for (int i = 1; i < a.length; i++) {
      if (less(a[i], a[i-1])) {
        return false;
      }
    }
    return true;
  }
}
```



## Selection sort: Java implementation



```Java
public class Selection
{
  public static void sort(Comparable[] a) {
    int N = a.length;
    for (int i = 0; i < N; i++) {
      int min = i;
      for(int j = i + 1; j < N; j++) {
        if (less(a[j], a[min])) {
          min = j;
        }
      }
      exch(a, i, min);
    }
  }
}
```



### Mathematical analysis

O(N^2)



## Reference

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/21ElementarySorts.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}

