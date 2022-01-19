# Lambda

作为从Java转Go的开发者，已经习惯Java的Lambda表达式方式。Go 下是否存在如下的实现。

## Functional

如下是一种通过匿名函数实现方式，下面是网上找的例子。

```go
func Test(n int, x string) {
    fmt.Println(n, x)
}
func main() {
    myFunc := func(x string) { Test(123, x) }
    myFunc("hello")
}
```

## Stream API

Stream API，我找到如下几种开源实现方式：

- https://github.com/vladimirvivien/automi
- https://github.com/jucardi/go-streams



