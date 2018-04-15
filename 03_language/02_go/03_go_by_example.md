# Go by Example

[TOC]

## hello world

```go
package main

import "fmt"

func main() {
    fmt.Println("hello world")
}

// go run foo.go
// go build foo.go
// ./foo
```

## values

> strings, integers, floats, booleans, etc

```go
package main

import "fmt"

func main() {
    
    fmt.println("go" + "lang")
    fmt.Println("1 + 1 = ", 1 + 1)
    fmt.Println("7.0 / 3.0 = ", 7.0 / 3.0)
    
    fmt.println(true && false)
    fmt.Println(true || false)
    fmt.Println(!true)
}
```

## variables

> 供类型检查或函数调用
>
> var 声明一个或多个变量
>
> go会推断类型的初始值
>
> 没有初始化的已声明变量会初始化为zero-valued.比如int型的是0
>
> ==:\=== 是变量声明和初始化的简写形式，`var f string = "short"`

```go
package main

import "fmt"

func main() {
    var a = "initial"
    fmt.Println(a)
    
    var b, c int = 1, 2
    fmt.Println(b, c)
    
    var d = true
    fmt.Println(d)
    
    var e int 
    fmt.Println(e)
    
    f := "short"
    fmt.Prinltn(f)
}
```

## constants

> go support constants of character, string, boolean and numberic values.
>
> const declares a constant value.
>
> a const statement can appear anywhere a var statement can.
>
> constant expressions perform arithmetic with arbitrary precision.
>
> a numberic constat has no type until it's given one , such by an explicit cast.
>
> a number can be given a type by using it in a context that requires one, such as a variable assignment or function all. for example
>
> here math.Sin expects a float64



```go
package main

import "fmt"
import "math"

const a string = "constant"

func main() {
    fmt.Println(a)
    
    const n = 5000000000
    const d = 3e20 / n
    
    fmt.Println(d)
    fmt.Println(int64(d))
    fmt.Println(math.Sin(n))
}
```



## for

> for is go's only looping construct. here are three basic types of for loops.
>
>  
>
> the most basic type, with a single condition
>
> a classic initial/condition/after for loop.
>
> for without a condition will oop repeatedly until you break out of the loop or return from the enclosing function.
>
> you can also continue to the next iteration of the loop.

```go
package main

import "fmt"

func main() {
    
    i := 1
    for i <= 3 {
        fmt.Println(i)
        i = i + 1
    }
    
    for j := 7; j <= 9; j++ {
        fmt.Println(j)
    }
    
    for {
        fmt.Println("loop")
        break
    }
    
    for n := 0; n <= 5; n++ {
        if n % 2 == 0 {
            continue
        }
        fmt.Println(n)
    }
}
```

## if/else

> branching with if and else in go is straight-forward.
>
> you can have an if statement without an else.
>
> a statement can precede conditionals; any variables declared in this statement are available in all branches.
>
> note that yo don't need parentheses around conditions in go, but that the braces are required.
>
> there is no ternary if in go, so you'll need to use a full if statement even for basic conditions.

```go
// if-else.go

package main

import "fmt"

func main() {
    if 7 % 2 == 0 {
        fmt.Println("7 is even")
    } else {
        fmt.Println("7 is odd")
    }
    
    if 8 % 4 == 0 {
        fmt.Println("8 is divisible by 4")
    }
    if num := 9; num < 0 {
        fmt.Println(num "is negative")
    } else if num < 10 {
        fmt.Println(num, "has 1 digit")
    } else {
        fmt.Println(num, "has multiple digits")
    }
}
```

## switch

> switch statements express conditions across many branches.
>
> you can use commas to separate multiple expressions in the same case statement. we use the optional default case in this example as well
>
> switch without an express is an alternative way to expression if / else logic. here we also show how the case expression
>
> can be non-constants.
>
> a type switch compares types instead of values. you can use this to discover the type of an interface value. in this example,
>
> the variable t will have the type corresponding to its clause.

```go
// switch.go

package main

import "fmt"
import "time"

func main() {
    
    i := 2
    fmt.Print("write ", i, " as ")
    switch i {
    case 1:
        fmt.Println("one")
    case 2:
        fmt.Println("two")
    case 3:
        fmt.Println("three")
    }
    switch time.Now().Weekday() {
    case time.Saturday, time.Sunday:
        fmt.Prinltn("it's the weekday")
    default:
        fmt.Println("it's a weekday")
    }
    
    t := timeNow()
    switch {
    case t.Hour() < 12:
        fmt.Println("it's before noon")
    default:
        fmt.Println("it's after noon")
    }
    
    whatAmI := func(i interface{}) {
        switch t := i.(type) {
        case bool:
            fmt.Println("i'm a bool")
        case int:
            fmt.Println("i'm an int")
        default:
            fmt.Println("don't know type %T\n", t)
        }
    }
    
    whatAmI(true)
    whatAmI(1)
    whatAmI("hey")
}
```

