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

### 前提

Giving N objects

### 目的

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

