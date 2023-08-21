---
title: Stacks
date: 2023-08-20T10:00:41+08:00
weight: 1
tags:
- Algorithm
- stacks
---

{{< toc >}}

## Stack Api

---

Warmup API. Stack of string data type.

這裡會建立如下的Stack API

```
public clacc StackOfStrings

  StackOfStrings() // create am empyt stack
  void push(String item) // insert a new strig onto stack
  String pop() //remove and return the string most recently added
  booleam isEmpty() // is the stack empty?
  int size() // number of strings on the stack
```



### Linked-list implementation

將最新的 item 加到 Linked-list 的最前端，移除的時候也是從第一個開始移除

``` java
public class LinkedStackOfStrings
{
  private Node first = null;
  private class Node {
    String item;
    Node next;
  }
  
  public boolean isEmpty() {
    return first == null;
  }
  public void push(String item) {
    Node oldfirst = first;
    first = new Node();
    first.item = item;
    first.next = oldfirst;
  }
  public String pop()
  {
    String item = first.item;
    first = first.next;
    return item;
  }
}
```

### Array implementation

```Java
public class FixedCapacityStackOfStrings
{
  private String[] s;
  private int N = 0;
  
  public FixedCapacityStackOfStings(int capacity) {
    s = new String[capacity];
  }
  public boolean isEmpty() {
    return N == 0;
  }
  public void push(String item) {
    s[N++] = item; ' // 簡寫：表示先用index，用完之後將N increment
  }
  public String pop() {
    return s[--N]; // 表示先將Ｎ decrement; 再拿 array index 為 N的 元素
  }
}
```



### Stack considerations

#### Overflow and underflow.

- Underflow: throw exception if pop from an empty stack.
- Overflow: use resizing array for array implementation.

#### Null items

允許插入 null item

#### Loitering

Holding a reference to an object when it is no longer needed.

所以程式可以改寫以避免這種問題

```
public String pop()
{
  String item = s[--N];
  s[N] = null;
  return item;
}

```

Java 的 garbage collector 可以reclaim memory

## 參考來源

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/13StacksAndQueues.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}

