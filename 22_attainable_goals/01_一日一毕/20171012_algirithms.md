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



# 07 递归的高级应用

> 汉诺塔， 子树概念

```java
public class HanoiTower {
  /**
  *	topN: 移动的盘子数
  	from: 起始塔座
  	inter: 中间塔座
  	to: 目标塔座
  */	
  public static void doTower(int topN, char from, char inner, char to) {
    if (topN == 1) {
      System.out.println("盘子1, 从" + from + "塔座到" + to + "塔座");
    } else {
      doTower(topN - 1, from, to, inter);
      System.out.println("盘子" + topN + ", 从" + from + "塔座到" + to + "塔座");
      doTower(topN - 1, inter, from, to);
    }
  }
}
```



# 08 希尔排序

> 插入排序缺点: 如果很小的数据在靠右侧，那么要将该数据排序到正确的位置上，则所有的中间数据都需要向右移动一位

```java
public class ShellSort {
  public static void sort(long[] arr) {
    // 初始化一个间隔
    int h = 1;
    // 计算最大间隔
    while (h < arr.length / 3) {
      h = h * 3 + 1;
    }
    
    while (h > 0) {
      // 进行插入排序
      long tmp = 0;
      
      for (int i = h; i < arr.length; i++) {
        tmp = arr[i];
        int j = i;
        while (j > h - 1 && arr[j - h] >= tmp) {
          arr[j] = arr[j - h];
          j -= h;
        }
        arr[j] = tmp;
      }
      
      // 减小间隔
      h = (h - 1) / 3
    }
  }
}
```

# 09 快速排序

> 划分两个子数组，设定关键字，比关键字小的放一组，比关键字大的放另一组

```java
public static void partition(long arr[], int left, int right, int point) {
  int leftPtr = left - 1;
  int right = right + 1;
  while (true) {
    // 循环, 将比关键字小的留在左端
    while (leftPtr < rightPtr && arr[++leftPtr] < point);
    // 循环，将比关键字大的留在右端
    while (rightPtr > leftPtr && arr[--rightPtr] > point);
    if (leftPtr >= rightPtr) {
      return;
    }
    long tmp = arr[leftPtr];
    arr[leftPtr] = arr[rightPtr];
    arr[rightPtr] = tmp;
    
    return leftPtr;
  }
}

public static void sort(long[] arr, int left, int right) {
  if (right - left <= 0) {
    return;
  }
  // 设置关键字
  long point = arr[right];
  // 获得切入点，同时对数组进行划分
  int partition = partition(arr, left, right, point);
  // 对左边的子数组进行快速排序
  sort(arr, left, partition - 1);
  // 对右边的子数组进行快速排序
  sort(arr, partition + 1, right);
}
```

# 10 二叉树

> 有序数组插入和删除数据项太慢
>
> 链表查询数据太慢
>
> 树中能够很快的查找数据项，插入数据项和删除数据项



```java
public class Node {
  private long data; // 数据项
  private Node leftChild;
  private Node rightChild;
  
  public Node(long data) {
    this.data = data;
  }
}

public class Tree {
  // 根节点
  private Node root;
  // 插入节点
  public void insert(long value) {
    
  }
  // 查找节点
  public void find(long value) {
   
  }
  // 删除节点
  public void delete(long value) {
    
  }
}
```

# 11 二叉树的基本操作

```java
// 插入
public void insert(long value) {
  // 封装节点
  Node newNode = new Node(value);
  // 引用当前节点
  Node current = root;
  Node parent;
  if (root == null) {
    root = newNode;
    return;
  } else {
    while (true) {
      // 父节点指向当前节点
      parent = current;
      // 如果当前指向的节点数据比插入的要大，则向左走
      if (current.data > value) {
        current = current.leftChild;
        if (current == null) {
          parent.leftChild = newNode;
          return;
        }
      } else {
        current = current.rightChild;
        if (current == null) {
          parent.rightChild = newNode;
          return;
        }
      }
    }
  }
}


// 查找节点
public void find(long value) {
  // 引用当前节点，从根节点开始
  Node current = root;
  // 循环 只要查找值不等于当前节点的数据项
  while (current.data != value) {
    if (current.data > value) {
      current = current.leftChild;
    } else {
      current = current.rightChild;
    }
    // 如果查找不到
    if (current == null) {
      return null;
    }
  }
  return current;
}
```

# 12 遍历二叉树

> 前中后指相对访问节点的顺序
>
> 前序遍历 
>
> 1. 访问根节点
> 2. 前序遍历左子树
> 3. 前序遍历右子树
>
> 中序遍历
>
>  	1. 中序遍历左子树
>  	2. 访问根节点
>  	3. 中序遍历右子树
>
> 后序遍历
>
>  	1. 后序遍历左子树
>  	2. 后序遍历右子树
>  	3. 访问根节点

















