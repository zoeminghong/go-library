### 自定义类型

使用关键字type就可以开始自定义类型，包括基于现有基础类型创建，或者是结构体、函数类型等

```go
type name int8
```

多个type还可以合成一个组

```go
package main

import (
	"fmt"
)

type (
	user struct {
		name string
		age  uint8
	}
	event func(string) bool
)

func main() {
	u := user{"Jason", 20}
	fmt.Println(u)
	var f event = func(s string) bool {
		fmt.Println(s)
		return s != ""
	}
	f("abc")

}

```

`结果`

```go
{Jason 20}
abc
```

注意

```go
package main

import (
	"fmt"
)

type data int

func main() {
	var d data = 10
	var x int = d
	fmt.Println(x)
	fmt.Println(d == x)

}
```

> d虽然底层的数据类型是int，但其与int是两个不同的概念，不能作为相同的数据类型看待

**未命名类型**

与有明确标识符的bool、int、string等类型相比，数组、切片、字典、通道等类型与具体元素类型或长度等属性有关，故称作未命名类型。当然，可用type为其提供具体的名称，将其改变为命名类型

- 具有相同基类型的指针
- 具有相同元素类型和长度的数组
- 具有相同元素类型的切片
- 具有相同键值类型的字典
- 具有相同数据类型及操作方向的通道
- 具有相同字段序列（字段名、字段类型、标签、以及字段顺序）的结构体
- 具有相同签名（参数的返回值列表，不包括参数名）的函数
- 具有相同方法集（方法名、方法签名、不包括顺序）的接口

未命名类型转换规则

- 所属类型相同
- 基础类型相同，且其中一个是未命名类型
- 数据类型相同，将双向通道给单向通道，且其中一个为未命名类型
- 将默认值nil赋值给切片、字典、通道、指针、函数或接口
- 对象实现了目标接口

```go
package main

import (
	"fmt"
)

type data [2]int

func main() {
	var d data = [2]int{1, 2}
	fmt.Println(d)
	a := make(chan int, 2)
	var b chan<- int = a
	b <- 2

}
```

`结果`

```go
[1 2]
```