# Go Web编程

[TOC]



## Go环境配置

### 获取远程包

```shell
go get github.com/astaxie/beedb
```

### 使用远程包

```go
// go get本质上可以理解为首先第一步是通过源码工具clone代码到src下面，然后执行go install，使用远程包因此和使用本地包一样
import "github.com/astaxie/beedb"
```

### 清除编译文件



### go install (生成结果文件, 编译好的结果文件移动到GOPATH/pkg或bin下)

### go test *_test.go文件

### go doc net/http (go doc fmt Printf) (godoc -src fmt Printf)

## Go语言基础

```go
package main

import "fmt"

func main() {
    fmt.Println("hello world 你好，世界 or \n")
}
```

### 定义变量

```go
var variableName type
var vname1, vname2, vname3 type
var vname1, vname2, vname3 type = v1, v2, v3
var vname1, vname2, vname3 = v1, v2, v3

// := 简短声明， 但只能用在函数内部
vname1, vname2, vname3 := v1, v2, v3


```

### 常量

```go
const constName = 3.1415926
```

### 分组声明

```go
import "fmt"
import "os"

const i = 100
const pi = 3.1415
const prefix = "go_"

var i int
var pi float32
var prefix string


// 可以分组写成如下形式
import (
	"fmt"
    "os"
)

const (
	i = 100
    pi = 3.1415
    prefix = "go_"
)

var (
	i int
    pi float32
    prefix string
)
```

### slice

- len 获取slice长度
- cap 获取slice的最大容量
- append向slice里面追加一个或者多个元素，然后返回一个和slice一样的类型的slice
- copy 函数copy



### map

```go
map[keyType]valueType

nums := make(map[string]int)
nums['one'] = 1
nums['ten'] = 10
nums['three'] = 3

fmt.Println("第三个数字是: ", nums['three'])
```



### 指针

Go语言中string, slice, map这三种类型的实现机制类似指针，所以可以直接传递，而不用取地址后传递指针

### defer

后进先出

```go
for i := 0; i < 5; i++ {
    defer fmt.Println("%d", i)
}
```



