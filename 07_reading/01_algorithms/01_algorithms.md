[TOC]

# 链表

## 1. 反转链表

```java
class ListNode {
    int val;
  	ListNode(int val) {
        this.val = val;
    }
}
public ListNode reverse(ListNode head) {
    ListNode currentNode = head;
  	ListNode prev = null;
  	while (currentNode != null) {
        ListNode next = currentNode.next;
      	currentNode.next = prev;
      	prev = currentNode;
      	currentNode = currentNode.next
    }
  	return prev;
}
```

## 2. 双向链表

```java

```

