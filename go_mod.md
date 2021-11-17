# GO Mod

配置环境变量，一种方式直接修改 `.bash_profie` 文件，还有一种是下面的命令方式。

```
export GOPATH="/usr/local/go"
export GOROOT="/usr/local/go/"
export GOBIN="/usr/local/go/bin"
export PATH="$GOROOT/bin:$GOPATH::$PATH"
```

```routeros
#修改 GOBIN 路径（可选）
go env -w GOBIN=$HOME/bin
#打开 Go modules
go env -w GO111MODULE=on
#设置 GOPROXY,开启依赖下载加速
go env -w GOPROXY=https://goproxy.cn,direct
```

在使用模块的时候， `GOPATH` 是无意义的，不过它还是会把下载的依赖储存在 `GOPATH/src/mod` 中，也会把 `go install` 的结果放在 `GOPATH/bin`（如果 `GOBIN` 不存在的话）

- `go mod download` 下载模块到本地缓存，缓存路径是 `$GOPATH/pkg/mod/cache`
- `go mod edit` 是提供了命令版编辑 `go.mod` 的功能，例如 `go mod edit -fmt go.mod` 会格式化 `go.mod`
- `go mod graph` 把模块之间的依赖图显示出来
- `go mod init` 初始化模块（例如把原本dep管理的依赖关系转换过来）
- `go mod tidy` 增加缺失的包，移除没用的包
- `go mod vendor` 把依赖拷贝到 `vendor/` 目录下
- `go mod verify` 确认依赖关系
- `go mod why` 解释为什么需要包和模块

```
// 初始化mod，会生成一个 go.mod 文件
go mod init [项目路径]
// 下载并添加依赖到go.mod文件中
go build, go test
// 查看module下的所有依赖
go list -m all
// 更新稳定版依赖
go get [rsc.io/sampler]
// 指定版本依赖
go list -m -versions rsc.io/sampler
rsc.io/sampler v1.0.0 v1.2.0 v1.2.1 v1.3.0 v1.3.1 v1.99.99
go get rsc.io/sampler@v1.3.1
// 清理无用的依赖
go mod tidy
```

**Relation:** https://www.codeleading.com/article/18321671970/