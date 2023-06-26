---
title: Quick Find
geekdocFlatSection: true
weight: 2

resources:
- name: quick-find-example
  src: "quick-find-example.png"
  title: Quick Find example
  params:
    credits: "[ROBERT SEDGEWICK](https://sedgewick.io/) | [KEVIN WAYNE](https://www.cs.princeton.edu/people/profile/wayne) on [Algorithms in Java, 4th Edition](https://algs4.cs.princeton.edu/lectures/keynote/15UnionFind.pdf)"
- name: quick-union-example
  src: "quick-union-example.png"
  title: Quick union example
  params:
    credits: "[ROBERT SEDGEWICK](https://sedgewick.io/) | [KEVIN WAYNE](https://www.cs.princeton.edu/people/profile/wayne) on [Algorithms in Java, 4th Edition](https://algs4.cs.princeton.edu/lectures/keynote/15UnionFind.pdf)"
---
## 初始資料結構

- 名為 `id[]` 且數量為 N 的陣列

- `id[p]` 是物件 p 所屬於的 component id



{{< img name="quick-find-example" size="large">}}

以上圖為例，`id[6] = 0`



## Find

p 的 id 為？



## Connected.

p 跟 q 是否有相同的 id？

ex: `id[6] = 0, id[7] =1`, 6 跟7 並沒有connect



## Union.

要將包含 p 跟 q 的兩個components 合併，找出所有id 為 id[p] 的物件， 將其 id 修改成 id[q].

{{< img name="quick-union-example" size="large">}}

如上圖，在union 6跟1之後，所有原本 `id = id[6]` 的id值變為 `id[1]`



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




