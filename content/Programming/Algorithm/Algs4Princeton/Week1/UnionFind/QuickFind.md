---
title: Quick Find
date: 2023-06-26T10:00:41+08:00
weight: 2
tags:
- Algorithm
---

{{< toc >}}

## Quick Find Structure

### 初始資料結構

- 名為 `id[]` 且數量為 N 的陣列

- `id[p]` 是物件 p 所屬於的 component id



![quick-find-example](https://raw.githubusercontent.com/viccj/upic/master/uPic/quick-find-example.png)

以上圖 [<sup>[1]</sup>](#refer-anchor-1)為例，`id[6] = 0`



### Find

p 的 id 為？



### Connected.

p 跟 q 是否有相同的 id？

ex: `id[6] = 0, id[7] =1`, 6 跟7 並沒有connect



### Union.

要將包含 p 跟 q 的兩個components 合併，找出所有id 為 id[p] 的物件， 將其 id 修改成 id[q].

![quick-union-example](https://raw.githubusercontent.com/viccj/upic/master/uPic/quick-union-example.png)


如上圖 [<sup>[1]</sup>](#refer-anchor-1)，在union 6跟1之後，所有原本 `id = id[6]` 的id值變為 `id[1]`



## Implemetion

```Java
public class QuickUF {
    
    private int[] id;
    
    public QuickFindUF(int N) {
        for (int i = 0; i < N; i++) {
    	    id[i] = i;
    	}
    }
    
    public int find(int p) {
    	return id[p];
    }
    
    public void union(int p, int q) {
    	int pId = id[p];
    	int qid = id[q];
    	for (int i = 0; i < id.length; i++) {
            if (id[i] == pid) {
                id[i] = qid;
            }
    	}
    }
  
}
```

## Too slow

### Cost model

| Algorithm  | initialize | union | Find |
| ---------- | ---------- | ----- | ---- |
| Quick-find | N          | N     | 1    |



#### Quick-find defect: Union too expensive

Ex: 對於N個物件來說，如果有N個Union 指令，那會需要N<sup>2</sup>次

因此要想辦法提高Union的效率


## 參考來源

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/15UnionFind.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}


