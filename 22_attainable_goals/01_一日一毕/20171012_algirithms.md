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

前提是数组必须是有序的

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

## 02  选择排序

## 03 插入排序

























