---
title: Dynamic Connectivity
date: 2023-06-24T10:00:41+08:00
weight: 1
resources:
- name: Dynamic-connectivity-example
  src: "DynamicConnectivityExample.png"
  title: Dynamic connectivity example
  params:
    credits: "[Robert Sedgewick](https://www.coursera.org/instructor/~250165) | [KEVIN WAYNE](https://www.coursera.org/instructor/~246867) on [Algorithms](https://d3c33hcgiwev3.cloudfront.net/_b65e7611894ba175de27bd14793f894a_15UnionFind.pdf?Expires=1687737600&Signature=ibQ5L3KCmNZqZ0-6PS6EQ9GitVnR6V9~lSEmgU4Tku2ayd7mCBMo4yUKPTRtcl6bAH5OmB0H6KPcIWUrObqS8AqvErqcO9wjEuZZ3dpdP0neG9qBKvk9nqShhAna6H4iSx34KPv0J6iKu-ZdDUwxFo9ve1RbGCriRekIchJHSMw_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A)"
---

{{< toc >}}

## 前提

Giving N objects

## 目的

寫出演算法來完成以下兩點

- Union command: connect two objects
- Find/connected query: Is there a path connecting the two objects?

假設給定的物件為 0,1,2,3,4,5,6,7,8,9，在執行完以下的`union`命令後，

```
Union(4, 3)
Union(3, 8)
Union(6, 5)
Union(9, 4)
Union(2, 1)
connected(0, 7) // no connected
connected(8, 9) // connected
union(5, 0)
union(7, 2)
connected(0, 7)
union(1, 0) // connected
```
connectivity 的狀況會如下圖

{{< img name="Dynamic-connectivity-example" size="large" lazy=false >}}

## 相關應用

- 數位照片的Pixels
- 在同一個網路的電腦
- 社群網站的好友



## 連接的定義

- Reflexive: p 跟 p 自己連接
- Symmetric: 若p跟q連接，q也跟p連接
- Transitive: 若p跟q連接，q跟r連接，則p跟r連接



物件如果連接可以想成他們是在同一個component裏面，因此就剛剛的例子，會有2個component set

```
{0,1,2,5,6,7} {3,4,8,9}
```



## 演算法實作

- Find query: 檢查兩個物件是不是在相同的components
- Union command: 將兩個物件所在的component取聯集



### Union-find data type (API)

### 目標：為了union-find 設計一個有效率的資料結構

其中

- Number of object N can be huge.
- Number of operation M can be huge.
- Find queries and union commands may be intermixed(交錯進行).



API 設計如下

```
public class UF

---

UF(int N) // initialize N sites with integer names (0 to N-1)

void union(int p, int q) // add connection between p and q

int find(int p) // component identifier for p (0 to N-1)

boolean connected(int p, int q) // return true if p and q are in the same component

int count // number of components


```

### Union-find implementation

```java
public class UF {

    private int[] id;  // access to component id (site indexed)
    private int count; // number of components

    public UF(int n) {
        // Initialize component id array
        count = N;
        id = new int[N];
        for (int i = 0; i < N; i++) {
            id[i] = i;
        }
    }

    public int count() {
        return count;
    }

    public boolean connected(int p, int q) {
        return find[p] == find[q];
    }

    public int find(int p) {
      return id[p];  
    }
    
    public void union(int p, int q) {
        // Put p and q into the same component.
        int pID = find(p);
        int qID = find(q);
        
        // Nothing to do if p and q are already in the same component.
        if (pID == qID) return;
        
        // Rename p’s component to q’s name.
        for (int i = 0; i < id.length; i++) {
            if (id[i] == pID) id[i] = qID;
            count--; 
        }
            
    }


    public static void main(String[] args) {
        // Read number of sites.
        int n = StdIn.readInt();
        
        // Initialize N components.
        UF uf = new UF(n);
        while (!StdIn.isEmpty()) {
            int p = StdIn.readInt();
            
            // Read pair to connect.
            int q = StdIn.readInt();
            
            // Ignore if connected.
            if (uf.find(p) == uf.find(q)) continue;
            
            // Combine components
            uf.union(p, q);
            
            // Print connections.
            StdOut.println(p + " " + q);
        }
        StdOut.println(uf.count() + " components");
    }
}
```