---
title: Queues
date: 2023-08-21T07:00:41+08:00
weight: 3
tags:
- Algorithm
- stacks
---

{{< toc >}}

## Queue Api

---

Just like stack api, only name different

```
public clacc QueueOfStrings

		QueueOfStrings() // create am empyt queue
	void enqueue(String item) // insert a new strig onto queue
	String dequeue() //remove and return the string most recently added
	booleam isEmpty() // is the queue empty?
	int size() // number of strings on the queue
```



### Linked-list implementation

有兩個指針，分別指向第一個以及最後一個 node

insert 新 item在最後面，remove item在最前面

``` java
public class LinkedQueueOfStrings
{
  private Node first, last
  private class Node {
    String item;
    Node next;
  }
  
  public boolean isEmpty() {
    return first == null;
  }
  public void enqueue(String item) {
		Node oldlast = last;
    last = new Node();
    last.item = item;
    last.next = null;
    if (isEmpty()) {
      first = last;
    } else {
      oldlast.next = last;
    }
    
  }
  public String dequeue()
  {
		String item = first.item;
    first = first.next;
    if (isEmpty()) {
      last = null;
    }
    return item;
  }
}
```

> Suppose that you implement a queue using a null-terminated singly-linked list, maintaining a reference to the item least recently added (the front of the list) but not maintaining a reference to the item most recently added (the end of the list). What are the worst-case running times for *enqueue* and *dequeue*?

Ans. Linear time for enqueue and constant time for dequeue.

單鏈表的話沒有 last 指針，要找到最後一個元素要遍歷整個linked list


## 參考來源

<div id="refer-anchor-1"></div>

[1] {{<ref-out href="https://algs4.cs.princeton.edu/lectures/keynote/13StacksAndQueues.pdf">}} Algorithms in Java, Lecture slide{{</ref-out>}}


