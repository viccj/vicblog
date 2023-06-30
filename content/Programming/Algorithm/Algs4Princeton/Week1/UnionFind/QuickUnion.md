---
title: Quick Union
date: 2023-06-30T10:00:41+08:00
weight: 3
tags:
- Algorithm
---

{{< toc >}}

因為 Quick-find 演算法中，Union 的效率太低，因此要改善Union，在這章節稱為Quick Union （lazy approach），為什麼要叫Lazy approach呢？因為能躺著就不要做著，能坐著就不要站著，除非死到臨頭不然不要行動。



## 初始資料結構

- 名為 `id[]` 且數量為 `N` 的陣列

- `id[p]` 是物件 `p` 所屬於的`component id`

上面兩個與 Quick-find 一樣，但這次多了一個新的結構，` root`



### Root

將一整個Array 看成 a set of trees，也就是說每個物件都屬於一個 tree，這個資料結構就是由很多 tree 集合而成。



- 每個物件，會有一個Parent，如下圖，（3 可以接到4，3的 parent 是 4，4 的 parent 是 9)

![my-quick-union-ds](https://raw.githubusercontent.com/viccj/upic/master/uPic/my-quick-union-ds.png)


- 順著線一直找，當找不到 parent 時，這時候就稱這點為 root

- 如果這個物件只有自己，那自己就是root，例如1



### Find

因為每個物件都會有一個 root，要找兩個物件是不是同一個 component 的話，只要看他們的 root 是不是一樣就可以



### Union

要將兩個物件（例如p, q) 合併，只要將 p 的 root 的 id 變成 q 的root的 id 就可以

下圖為將9 跟6 合併的例子

![my-qu-exam](https://raw.githubusercontent.com/viccj/upic/master/uPic/my-qu-exam.png)

這時候只需要把`id[9]`改成6 就可以完成合併了

![qu-last-one-i](https://raw.githubusercontent.com/viccj/upic/master/uPic/qu-last-one-i.png)


## Implement

``` Java
public class QuickUnionUF {
    private int[] id;
    
    // set id of each object to itself (N array access)
    public QuickFindUF(int N) {
        id = new int[N];
        for (int i = 0; i < N; i++) {
    	    id[i] = i;
    	}
    }
    
    // in the book, this is find in p224
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
    	id[i] = j
    }
  
}
```



## Also too slow

### Cost model

| Algorithm   | initialize | union | Find |
| ----------- | ---------- | ----- | ---- |
| Quick-find  | N          | N     | 1    |
| Quick-union | N          | N     | N    |



### Quick-find defect

- Union too expensive： 對於 N 個物件來說，如果有 N 個 Union 指令，那會需要執行 N<sup>2</sup> 次，因此要想辦法提高 Union 的效率

### Quick-union defect

-  Tree can be tall： 導致`Find` 效率太差



## 參考來源

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/15UnionFind.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}


