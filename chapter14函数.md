### 函数

go语言至少有一个main函数

```go
func funcName(input1 type1, input2 type2) (output1 type1, output2 type2) {
//这里是处理逻辑代码
//返回多个值
return value1, value2
}
```

- func：函数由 func 开始声明
- funcName：函数名称，函数名和参数列表一起构成了函数签名。
- input1 type1, input2 type2：参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数。
- output1 type1, output2 type2：返回类型，函数返回一列值。return_types 是该列值的数据类型。有些功能不需要返回值，这种情况下 return_types 不是必须的。
- 上面返回值声明了两个变量output1和output2，如果你不想声明也可以，直接就两个类型。
- 如果只有一个返回值且不声明返回值变量，那么你可以省略包括返回值的括号（即一个返回值可以不声明返回类型）
- 函数体：函数定义的代码集合。

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int = 200
   var ret int

   /* 调用函数并返回最大值 */
   ret = max(a, b)

   fmt.Printf( "最大值是 : %d\n", ret )
}

/* 函数返回两个数的最大值 */
func max(num1, num2 int) int {
   /* 定义局部变量 */
   var result int

   if (num1 > num2) {
      result = num1
   } else {
      result = num2
   }
   return result 
}
```

`结果`

```go
最大值是 : 200
```

> Go 函数可以返回多个值

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
   return y, x
}

func main() {
   a, b := swap("Mahesh", "Kumar")
   fmt.Println(a, b)
}
```

```go
func SumAndProduct(A, B int) (add int, Multiplied int) {
add = A+B
Multiplied = A*B
return
}
```

Go函数支持变参。接受变参的函数是有着不定数量的参数的。为了做到这点，首先需要定义函数使其接受变参：

```go
func myfunc(arg ...int) {}
```

`arg ...int`告诉Go这个函数接受不定数量的参数。注意，这些参数的类型全部是int。在函数体中，变量arg是一
个int的slice：

```go
for _, n := range arg {
fmt.Printf("And the number is: %d\n", n)
}
```

go语言函数的参数也是存在**值传递**和**引用传递**

函数运用场景

**值传递**

```go
package main

import (
   "fmt"
   "math"
)

func main(){
   /* 声明函数变量 */
   getSquareRoot := func(x float64) float64 {
      return math.Sqrt(x)
   }

   /* 使用函数 */
   fmt.Println(getSquareRoot(9))

}
```

**引用传递**

这就牵扯到了所谓的指针。我们知道，变量在内存中是存放于一定地址上的，修改变量实际是修改变量地址处的内
存。只有add1函数知道x变量所在的地址，才能修改x变量的值。所以我们需要将x所在地址&x传入函数，并将函数的参数的类型由int改为*int，即改为指针类型，才能在函数中修改x变量的值。此时参数仍然是按copy传递的，只是copy的是一个指针。请看下面的例子

```go
package main
import "fmt"
//简单的一个函数，实现了参数+1的操作
func add1(a *int) int { // 请注意，
*a = *a+1 // 修改了a的值
return *a // 返回新值
} f
unc main() {
x := 3
fmt.Println("x = ", x) // 应该输出 "x = 3"
x1 := add1(&x) // 调用 add1(&x) 传x的地址
fmt.Println("x+1 = ", x1) // 应该输出 "x+1 = 4"
fmt.Println("x = ", x) // 应该输出 "x = 4"
}
```

- 传指针使得多个函数能操作同一个对象。
- 传指针比较轻量级 (8bytes),只是传内存地址，我们可以用指针传递体积大的结构体。如果用参数值传递的话, 在每次copy上面就会花费相对较多的系统开销（内存和时间）。所以当你要传递大的结构体的时候，用指针是一个明智的选择。
- **Go语言中slice，map这三种类型的实现机制类似指针**，所以可以直接传递，而不用取地址后传递
  指针。（注：若函数需改变slice的长度，则仍需要取地址传递指针）

**闭包**

Go 语言支持匿名函数，可作为闭包。匿名函数是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。

```go
package main

import "fmt"

func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   /* nextNumber 为一个函数，函数 i 为 0 */
   nextNumber := getSequence()  

   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   
   /* 创建新的函数 nextNumber1，并查看结果 */
   nextNumber1 := getSequence()  
   fmt.Println(nextNumber1())
   fmt.Println(nextNumber1())
}
```

`结果`

```go
1
2
3
1
2
```

**函数做为值**

在Go中函数也是一种变量，我们可以通过type来定义它

**同种方法：**参数类型、个数、顺序相同，返回值相同

```go
package main
import "fmt"
type testInt func(int) bool // 声明了一个函数类型
func isOdd(integer int) bool {
  if integer%2 == 0 {
  	return false
  } 
  return true
}
func isEven(integer int) bool {
  if integer%2 == 0 {
  return true
  }
  return false
}

func filter(slice []int, f testInt) []int {
  var result []int
  for _, value := range slice {
    if f(value) {
    	result = append(result, value)
    }
  }
  return result
} 
func main(){
  slice := []int {1, 2, 3, 4, 5, 7}
  fmt.Println("slice = ", slice)
  odd := filter(slice, isOdd) // 函数当做值来传递了
  fmt.Println("Odd elements of slice are: ", odd)
  even := filter(slice, isEven) // 函数当做值来传递了
  fmt.Println("Even elements of slice are: ", even)
}
```

`type testInt func(int) bool`就是将该种函数类型赋值给testInt

**一般步骤：**

1. 定义一个函数类型
2. 实现定义的函数类型
3. 作为参数调用

有点接口的感觉

> 函数当做值和类型在我们写一些通用接口的时候非常有用，通过上面例子我们看到testInt这个类型是一个函数类型，然后两个filter函数的参数和返回值与testInt类型是一样的，但是我们可以实现很多种的逻辑，这样使得我们的程序变得非常的灵活
>

**Panic和Recover**
Go没有像Java那样的异常机制，它不能抛出异常，而是使用了panic和recover机制。一定要记住，你应当把它作为最后的手段来使用，也就是说，你的代码中应当没有，或者很少有panic的东西。这是个强大的工具，请明智地使用它。那么，我们应该如何使用它呢？
`Panic`
是一个内建函数，可以中断原有的控制流程，进入一个令人恐慌的流程中。当函数F调用panic，函数F的执行被中
断，但是F中的延迟函数会正常执行，然后F返回到调用它的地方。在调用的地方，F的行为就像调用了panic。这一过程继续向上，直到发生panic的goroutine中所有调用的函数返回，此时程序退出。恐慌可以直接调用panic产
生。也可以由运行时错误产生，例如访问越界的数组。
`Recover`
是一个内建的函数，可以让进入令人恐慌的流程中的goroutine恢复过来。recover仅在延迟函数中有效。在正常
的执行过程中，调用recover会返回nil，并且没有其它任何效果。如果当前的goroutine陷入恐慌，调用
recover可以捕获到panic的输入值，并且恢复正常的执行。

下面这个函数演示了如何在过程中使用panic

```go
var user = os.Getenv("USER")
  func init() {
    if user == "" {
    	panic("no value for $USER")
    }
}	
```

下面这个函数检查作为其参数的函数在执行时是否会产生panic：

```go
func throwsPanic(f func()) (b bool) {
  defer func() {
    if x := recover(); x != nil {
       b = true
    }
  }()
	f() //执行函数f，如果f中出现了panic，那么就可以恢复回来
	return
}
```