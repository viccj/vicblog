---
title: Resizing Arrays
date: 2023-08-20T10:00:41+08:00
weight: 2
tags:
- Algorithm
---
{{< toc >}}

### Resizing-array implementation

#### Problem

如何增加或減少 array 的 size?

#### First try

- `push() `: increase size of array `s[]` by 1
- `pop() `: decrease size of array `s[]` by 1

#### Too expensive

- 每次新增都要複製舊的 array 到全新的 array
- push N 個 item 的時間複雜度 是1 + 2+...+N ~ N&sup2;/2

#### Another way

If array is full, create a new array of twice the size, and copy items.

```
public ResizingArrayStackOfStrings() {
  s = new String[1];
}

public void push(String item) {
  if (N == s.length) {
    resize(2 * s.length);
    s[N++] = item;
  }
private void resize(int capacity) {
  String[] copy = new String[capacity];
  for (int i = 0; i < N; i++) {
    copy[i] = s[i]
  }
  s = copy;
}
```

這樣插入 N 個 items 所花費的時間是 N （不是N&sup2;）

N + (2 + 4 + 8 + N) ~ 3N



### How to shrink array

#### First try

- `push() `: double size of array` s[] `when array is full
- `pop() `: halve size of array  `s[]` when array is one-half full

too expensive in worst case: pop.. push.. pop.. push..

#### Effectient solution

- `push() `: double size of array` s[] `when array is full
- `pop() `: halve size of array  `s[]` when array is one-quarter full

```
public string pop()
{
  String item = s[--N];
  s[N] = null;
  if(N > 0 && N == s.lenght/4) {
    resize(s.lenght/2);
    return item;
  }
}
```



### resizing array vs. linked list

#### Linked-list

- Every operation take constant time in the worst case.
- Uses extra time and space to deal with the links.

#### Resizing array

- Every operation takes constant amortized time.
- Less wasted space



> 將一個操作的時間複雜度描述為「constant amortized time」，意味著在一系列操作中，每個操作雖然在某些情況下可能需要更多時間，但平均情況下，每個操作的耗時都是固定的。這就是所謂的「攤銷分析」。在這種分析下，大多數操作的時間消耗都是固定的，但可能有一小部分操作需要較長時間。通過攤銷分析，我們可以確保整體平均下來，每個操作的時間複雜度仍然保持在常數水平
> 
> 
> ## 參考來源

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/13StacksAndQueues.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}

