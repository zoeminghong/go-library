### 变量与常量

#### 变量与常量的定义

> 以字母或下划线开头，由一个或多个字母、数字、下划线组成

#### 变量声明

第一种，指定变量类型，声明后若不赋值，使用默认值

```go
var name type
name = value
```

第二种，根据值自行判定变量类型

```go
var name = value
```

第三种，省略var, 注意 :=左侧的变量不应该是已经声明过的，否则会导致编译错误

```go
name := value

// 例如
var a int = 10
var b = 10
c : = 10
```

> 这种方式它只能被用在函数体内，而不可以用于全局变量的声明与赋值

`示例`

```go
package main
var a = "Hello"
var b string = "World"
var c bool

func main(){
    println(a, b, c)
}
```

`结果`

```go
Hello World false
```

#### 多变量声明

第一种，以逗号分隔，声明与赋值分开，若不赋值，存在默认值

```go
var name1, name2, name3 type
name1, name2, name3 = v1, v2, v3
```

第二种，直接赋值，下面的变量类型可以是不同的类型

```go
var name1, name2, name3 = v1, v2, v3
```

第三种，集合类型

```go
var (
    name1 type1
    name2 type2
)
```

#### 注意事项

如果在相同的代码块中，我们不可以再次对于相同名称的变量使用初始化声明，例如：a := 20 就是不被允许的，编译器会提示错误 no new variables on left side of :=，但是 a = 20 是可以的，因为这是给相同的变量赋予一个新的值。

如果你在定义变量 a 之前使用它，则会得到编译错误 undefined: a。

如果你声明了一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误，例如下面这个例子当中的变量 a：

```go
func main() {
   var a string = "abc"
   fmt.Println("hello, world")
}
```

尝试编译这段代码将得到错误 a declared and not used

此外，单纯地给 a 赋值也是不够的，这个值必须被使用，所以使用

**空白标识符 _ 也被用于抛弃值**，如值 5 在：_, b = 5, 7 中被抛弃

_ 实际上是一个只写变量，你不能得到它的值。这样做是因为 Go 语言中你必须使用所有被声明的变量，但有时你并不需要使用从一个函数得到的所有返回值

并行赋值也被用于当一个函数返回多个返回值时，比如这里的 val 和错误 err 是通过调用 Func1 函数同时得到：val, err = Func1(var1)

#### 常量声明

常量是一个简单值的标识符，在程序运行时，不会被修改的量。

常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型

```go
const identifier [type] = value
```

```go
显式类型定义： const b string = "abc"
隐式类型定义： const b = "abc"
```

```go
package main

import "fmt"

func main() {
   const LENGTH int = 10
   const WIDTH int = 5   
   var area int
   const a, b, c = 1, false, "str" //多重赋值

   area = LENGTH * WIDTH
   fmt.Printf("面积为 : %d", area)
   println()
   println(a, b, c)   
}
```

`结果`

```go
面积为 : 50
1 false str
```

常量可以作为枚举

```go
const (
    Unknown = 0
    Female = 1
    Male = 2
)
```
### iota

iota，特殊常量，可以认为是一个可以被编译器修改的常量

> 在每一个const关键字出现时，被重置为0，然后再下一个const出现之前，每出现一次iota，其所代表的数字会自动增加1

iota 可以被用作枚举值：

```go
const (
    a = iota
    b = iota
    c = iota
)
```

第一个 iota 等于 0，每当 iota 在新的一行被使用时，它的值都会自动加 1；所以 a=0, b=1, c=2 可以简写为如下形式：

```go
const (
    a = iota
    b
    c
)
```

**iota 用法**

```go
package main

import "fmt"

func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}
```

`结果`

```
0 1 2 ha ha 100 100 7 8
```
