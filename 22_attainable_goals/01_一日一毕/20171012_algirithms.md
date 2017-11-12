[TOC]

# 01 数组

## 01 查找数据

```java
public int search(long value) {
    for (int i = 0; i < elements; i++) {
        if (arr[i] == value) {
            return i;
        }
    }
  	return -1;
}
```



## 02 根据索引查找数据

```java
public long get(int index) {
    if (index >= elements || index < 0) {
        throw new ArrayIndexOutOfBoundException();
    }
  	return arr[index];
}
```

## 03 添加数据

```java
public void insert(long value) {
    arr[elements] = value;
  	elements++;
}
```

## 04 显示数据

```java
public void display() {
    System.out.println("[]");
  	for (int i = 0; i < elements; i++) {
        System.out.println(arr[i] + " ");
    }
  	System.out.println("]");
}
```

## 05 删除数据

```java
public void delete(int index) {
    if (index >= elements || index < 0) {
        throw new ArrayIndexOutOfBoundException();
    }
  	for (int i = index; i < elements; i++) {
        arr[i] = arr[i] + 1;
    }
  	elements--;
}
```

## 06 更新数据

```java
public void change(int index, int newValue) {
    if (index >= elements || index < 0) {
        throw new ArrayIndexOutOfBoundException();
    }
  	arr[index] = newValue;
}
```

## 07 有序数组

添加时保证有序, 数据从小到大排列

```java
public void insert(int value) {
    int i = 0;
  	for (int i = 0; i < elements; i++) {
        if (arr[i] > value) {
            break;
        }
    }
  	for (int j = elements; j > i; j--) {
        arr[j] = arr[j - 1];
    }
  	arr[i] = value;
  	elements++;
}
```

## 08 线性查找算法

```java
public int search(long value) {
    int i = 0;
  	for (int i = 0; i < elements; i++) {
        if (value = arr[i]) {
            break;
        }
    }
  	if (i = elements) {
        return -1;
    } else {
        return i;
    }
}
```

## 09 二分法查找

前提是数组必须是有序的, 每次规模折半

```java
public int search(long value) {
    int sup = elements, sub = 0;
  	while (sub <= sup) {
      int middle = (sup + sub) / 2;
      if (value == arr[middle]) {
          return middle;
      } else if (value < arr[middle]) {
          sup = middle - 1;
      } else if (value > arr[middle]) {
          sub = middle + 1;
      }      
    }
  	return -1;
}

public int binarySearch(int value) {
    int middle = 0;
  	int low = 0;
  	int pow = elements;
  	
  	while (true) {
      	int middle = (pow + low) / 2;
        if (arr[middle] == value) {
            return middle;
        } else if (low > pow) {
            return -1;
        } else {
            if (arr[middle] > value) {
                pow = middle - 1;
            } else {
                low = middle + 1;
            }
        }
    }
}
```

# 02 简单排序

## 01 冒泡排序

```java
public static void sort(long arr) {
  long tmp = 0;
  for (int i = 0; i < arr.length - 1; i++) {
    for (int j = arr.length - 1; j > i; j--) {
      tmp = arr[j];
      arr[j] = arr[j - 1];
      arr[j - 1] = tmp;
    }
  }
}
```



## 02  选择排序

与冒泡算法相似，指针记住最小的, 少了中间不断的交换动作，只交换当前规模下的首项和最大(小)项

```java
public static void sort(long[] arr) {
  int k = 0;
  long tmp = 0;
  for (int i = 0; i < arr.length - 1; i++) {
    for (int j = i; j < arr.length; j++) {
      if (arr[j] < arr[k]) {
        k = j;
      }
    }
    tmp = arr[i];
    arr[i] = arr[k];
    arr[k] = tmp;
  }
}
```



## 03 插入排序

```java
public static void sort(long[] arr) {
  long tmp = 0;
  for (int i = 1; i < arr.length; i++) {
    tmp = arr[i];
    int j = i;
    while (j > 0 && arr[j - 1] >= tmp) {
      arr[j] = arr[j - 1];
      j--;
    }
    arr[j] = tmp;
  }
}
```

# 03 栈和队列

## 01 栈的构造和应用

底层是一个数组

```java
// 
private long[] arr;
private int top;

// 添加数据
public void push(int value) {
  arr[++top] = value;
}

// 移除数据
public void pop() {
  return arr[top--];
}

// 查看数据
public void peek() {
  return arr[top];
}

// 判断是否为空
public boolean isEmpty() {
  return top == -1;
}

// 判断是否满了
public boolean isFull() {
  return top == arr.length - 1;
}
```



## 02 队列的构造和应用

底层也是数组

```java
public class MyQueue {
  private long[] arr;
  private int elements;
  private int front;
  private int end;
  
  public MyQueue() {
    arr = new long[10];
    elements = 0;
    front = 0;
    end = -1;
  }
  
  public MyQueue(int maxsize) {
    arr = new long[maxsize];
    elements = 0;
    front = 0;
    end = -1;
  }
  
  // 添加数据, 从队尾插入
  public void insert(long value) {
    arr[++end] = value;
    elements++;
  }
  
  // 删除数据， 从队头删除
  public long remove() {
    return arr[front++];
  }
  
  // 查看队头
  public long peek() {
    return arr[front];
  }
  
  // 判断是否为空
  public boolean isEmpty() {
    return elements == 0;
  }
  
  // 判断是否满了
  public boolean isFull() {
    return elements == arr.length;
  }
}
```

循环队列

```java
public void insert(long value) {
  if (end == arr.length - 1) {
    end = -1;
  }
  arr[++end] = value;
  elements++;
}

public long remove() {
  long value = arr[front++];
  if (front == arr.length) {
    front == 0;
  }
  elements--;
  return value;
}
```

# 04 链表

```java
public class Node {
  public long data;
  public Node next;
  
  public Node(long value) {
    this.data = value;
    
  }
  
  // 显示方法
  public void display() {
    System.out.print(data + " ");
  }
}

public class LinkList {
  private Node first;
  public LinkList() {
    first = null;
  }
  
  // 插入一个节点，在头结点后进行插入
  public void insertFirst(long value) {
    Node node = new Node(value);
    if (first == null) {
      first = node;
    } else {
      node.next = first;
      first = node;
    }
    
  }
  
  // 删除一个节点，在头节点后进行删除
  public Node deleteFirst() {
    Node tmp = first;
    first = tmp.next;
    return tmp;
  }
  // 显示方法
  public void display() {
    Node current = first;
    while (current != null) {
      current.display();
      current = current.next;
    }
  }
  
  // 查找方法
  public Node find(long value) {
    Node current = first;
    while (current.data != value) {
      if (current.next == null) {
        return null;
      }
      current = current.next;
    }
    return current;
  }
  
  // 删除方法
  public Node delete(long value) {
    Node current = first;
    Node previous = first;
    while (current.data != value) {
      if (current.next == null) {
        return null;
      }
      previous = current;
      current = current.next;
    }
    if (current == first) {
      first = tmp.next;
    } else {
      previous.next = current.next;
    }
    
    return current;
  }
}
```

# 05 双端链表和双向链表

> 双端: 链表中保存着对最后一个链结点引用的链表 

```java
public class FirstLastLinkList {
  private Node first;
  
  public FirstLastLinkList() {
    first = null;
  }
  
  // 判断是否为空
  public boolean isEmpty() {
    return first == null;
  }
  // 从头部插入
  // 从头部删除
  // 
}
```

> 双向链表

```java
public class Node {
  public long data;
  public Node previous;
  public Node next;
}
```

# 06 递归的应用

```java
public class Recursion {
  
}
```





























