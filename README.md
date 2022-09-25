# Langgo Framework

Langgo是一款go语言开发应用的框架。在B站以视频的形式同步开发。视频地址：https://space.bilibili.com/401571418/channel/collectiondetail?sid=699075

## 目录

 - [安装](#安装)
 - [快速开始](#快速开始)
 - [开发视频](#开发视频)
 - [grpc](#grpc)
 - [mysql](#mysql)
 - [自定义组件](#自定义组件)
## 安装

基于go 1.19开发

1. 安装langgo
```
go get -u github.com/langwan/langgo
```

2. 导入

```
import "github.com/langwan/langgo"
```

## 快速开始

```go
package main

import (
	"github.com/langwan/langgo"
	"github.com/langwan/langgo/components/hello"
	"github.com/langwan/langgo/core/log"
)

func main() {
	langgo.Run(&hello.Instance{Message: "hello component"})
	log.Logger("component", "hello").Info().Msg(hello.GetInstance().Message)
}
```

## 开发视频

视频地址 https://space.bilibili.com/401571418/channel/collectiondetail?sid=699075

## grpc

grpc支持单机模式和etcd服务发现两种模式，可以参考examples/grpc/single和examples/grpc/etcd两个例子。

## mysql

参考 `examples/mysql`

mysql配置支持多个mysql账号，例如：

```yaml
mysql:
  main:
    dsn: main:123456@tcp(localhost:3306)/simple?charset=utf8mb4&parseTime=True&loc=Local
    conn_max_lifetime: 1h
    max_idle_conns: 1
    max_open_conns: 10
  order:
    dsn: order:123456@tcp(localhost:3306)/simple?charset=utf8mb4&parseTime=True&loc=Local
    conn_max_lifetime: 1h
    max_idle_conns: 1
    max_open_conns: 10

```

这样可以支持项目会拥有多个mysql数据库

```go
langgo.Run(&mysql.Instance{})
var one int
mysql.Main().Raw("SELECT 1").Scan(&one)
fmt.Println(one)
```

`mysql.Main()` 表示获取配置中`main`下的mysql配置，如果想获取`order`，需要使用 `mysql.Get("order")`


## 自定义组件

完全可以在自己的项目中实现自定义组件与使用`langgo`框架中的内置组件的方式一样，只需要您在自己的项目里拷贝出`hello`组件的代码，进行改造，然后在启动的时候放在`langgo.Run`里一起启动就可以了：

```go
langgo.Run(&my.Instance{})
```

组件的配置文件放在`app.yml`配置中就可以了，例如：
```yaml
my:
  message: my name is langgo
```

可以参考 examples/component/custom 这个例子