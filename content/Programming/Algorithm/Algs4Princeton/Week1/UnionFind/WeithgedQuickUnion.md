---
title: Weighted Quick Union
date: 2023-07-29T10:00:41+08:00
weight: 4
tags:
- Algorithm
---

{{< toc >}}

How to imporvements Quick-union?

上一篇中， Quick-Union 演算法中，最後可能因為每個tree長得太高，導致效率變差。

## Imporvement 1: weighting

- 在每次 Union 時，順帶記錄每棵樹的 size (number of objects)

- 在每次 Union 時，刻意將較小的樹連接到較大的樹下



![Weight-Quick-Union_demo](https://raw.githubusercontent.com/viccj/upic/master/uPic/Weight-Quick-Union_demo.png)

### Data Structure

與 quick-union 一樣，但多追蹤每棵樹的大小 (size of objects for roots)

### Find

與 quick-union 一樣

### Union

修改為：

- 將小的樹往大的樹接
- 更新 `sz[]` array

### Implement

``` Java
public class QuickUnionUF {
    private int[] id;
    private int[] sz; // size of objects for roots (site indexed)
    
    // set id of each object to itself (N array access)
    public WeightedQuickFindUF(int N) {
        id = new int[N];
        for (int i = 0; i < N; i++) {
    	    id[i] = i;
    	}
        // init sz of every objects
        sz = new int[N];
        for (int i = 0; i < N; i++) {
            sz[i] = 1;
        }
    }
    
    public int root(int i) {
		while (i != id[i] ) {
            i = id[i];
        }
        return i;
    }
    
    
    public boolean connected(int p, int q) {
        return root[p] == root[q];
    }
    
    public void union(int p, int q) {
    	int i = root[p];
    	int j = root[q];
        
        if (i == j) {
            return;
        }
        if (sz[i] < sz[j]) {
            id[i] = j;
            sz[j] += sz[i];
        } else {
            id[j] = i;
            sz[i] += sz[j];
        }
    	id[i] = j
    }
  
}
```



### Weighted quick-union analysis

#### Running time.

- Find:  takes time proportional to depth of *p* and *q*.
- Union: takes constant time, given roots.

#### Proposition

Depth of any node *x* is at most *lg N* (log- base-2 logarithm).



這裡一開始比較難以理解，教授的原話如下

> So, when the depth of x increases, the size of its tree at least doubles. So, that's the key because that means that the size of the tree containing x can double at most log N times because if you start with one and double log N times, you get N and there's only N nodes in the tree.

看了書之後我的理解是：

- Maximum height of a tree only increases when 2 trees with same height are merged.

- You can make maximum logN merges of such trees. (as you will have to keep dividing N with 2 to form i amount of such trees. At the example with N=8 below, you create equally sized trees to merge max 3 times.

- Therefore, depth of any node x is at most LogN. (due to max height)



假設N是8，每次union後會多一層，每個樹的 objects 數量最多變兩倍，這樣一來對任何一顆樹而言 union 到最後最多只有三層. (log8)，因此對有 N個 object 的集合來說，每一顆 node的深度最多就是 logN。可以大幅減少每個 node 的 depth。



| Algorithm   | initialize | union | Find |
| ----------- | ---------- | ----- | ---- |
| Quick-find  | N          | N     | 1    |
| Quick-union | N          | N     | N    |
| Withgted QU | N          | lg N  | Lg N |



## Can we impove it even further?

其實是可以的，而且只要多一行code。

概念是：每次在尋找root的時候，將找到的 children node 都連向 root，這樣可以使得最後的樹趨近於平整

``` Java
public int root(int i) {
    while (i != id[i] ) {
        id[i] = id[id[i]] // one more line of code
        i = id[i];
    }
    return i;
}
```


## 參考來源

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/15UnionFind.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}


