## 环境变量

### 1.GO111MODULE

- 有三个值off / on / auto(默认值)
  1. off 表示不会支持module功能，会使用vendor或者GOPATH模式来查找。
  2. on 表示会使用module功能，并且不支持GOPATH模式。
  3. auto 意味着当前目录或者在目录中包含`go.mod`文件夹的话，即使当前目前在`$GOPATH/src`下，也会开启module模式。

### 2.GOPROXY

- 作用是设置代理服务器，可以设置多个代理服务器以及direct。

  ```properties
  ## 设置代理下载的网址，direct表示如果下载失败则直连，代理网址有多个
  GOPROXY = https://goproxy.cn,direct 
  GOPROXY = https://goproxy.io,direct
  ```
```
  

### 3.GOSUMDB

- 用来校验你下载的库的哈希是否和官方的哈希值是否一样，避免被proxy修改了，一般要开启。

  ```properties
  GOSUMDB = "sum.golang.org"
```

### 4.GOPRIVATE

- 用来设置不使用代理的仓库，比如一些公司内部的仓库，GONOPROXY(以前变量)和GOPRIVATE功能一样。

  ```properties
  GOPRIVATE="git.garena.com"
  ```

### 5.go env -w

- 命令go env -w 可以为这些值设置为全局变量。

  风险：在开发环境设置没问题，但是没有同步到测试或生产环境，造成项目无法编译。

  措施：将这些环境变量写到配置文件如Makefile中。

### 6.变量相关

- go help module-auth了解GOSUMDB和GONOSUMDB变量。
- go help goproxy了解proxy的通讯协议。
- go help modules了解module的功能。
- go help module-get了解go get命令的变化。而go help gopath-get显示先前的基于GOPATH的go get功能。
- go help module-private了解私有库的设置。



## go mod命令

- **内容初始化**
  
  1. 一般新的项目，无论文件夹在哪里，首先go mod init初始化它，然后go mod tidy自动将引入的库加入到go.mod文件中。
2. 当更新项目依赖的时候，可以使用go get -u 拉取最新的提交，注意分支名，go get -u name@branch
  3. go mod vendor可以将依赖加到vendor中，完成最新依赖的更新。
  
- **命令参数**
  
  1. download:下载依赖库到本地缓存中，当前在$GOPAH/pkg/mod中
  2. edit: 命令行中编辑go.mod
  3. graph: 显示依赖库的关系，包括间接依赖库都会显示，它以行的方式显示，如果能以tree方式显示更好。
  4. init: 初始化当前文件夹为module方式的go项目，会生成go.mod
  5. tidy: 增加新的依赖，移除不使用的依赖库
  6. vendor: 将依赖库放入到vendor文件夹下，如果将来使用module方式，这个命令是不会使用的
  7. verify: 校验下载到本地缓存中的依赖库自下载后未被修改
  8. why: 解释为什么需要一个依赖库(肯定是有代码使用它了, go help mod why可以看它的详细帮助)
  
  

## go action工具

- **go get**
  1. 命令格式
     `go get [-d] [-t] [-u] [-v] [-insecure] [build flags] [packages]`

  2. 运行原理
     - 首先go get会寻找依赖库的最新的tag未release的版本(tagged release version), 如 v0.4.5 或 v1.2.3, 
     - 如果没有，则寻找pre版，如v0.0.1-pre1,
     - 如果都没有，寻找最新的commit版。
     - `go get`相当于`go get .`,它应用于当前文件夹对应的包，`go get -u`和`go get -u=patch`同样的处理。

  3. 指定库的版本以及分支
     `go get golang.org/x/text@v0.3.0`
     `go get golang.org/x/text@master`

  4. 命令参数
     - `-t` 表示同时下载test文件中的依赖。
     - `-u` 表示下载的库使用最新的minor或patch最新版。
     - `-u=patch` 表示更新pacth release。
     - `go get xxxxx/...` 可以批量安装工具。
     - `go get golang.org/x/perf/cmd/...` 会安装perf的所有的命令行工具到$GOPATH/bin中。

- **go list**
    1. **命令格式**
       `go list [-f format] [-json] [-m] [list flags] [build flags] [packages]`

      2. **命令参数**
       - `-f` 指定要显示的字段， 
       - `-json` 指定显示的格式为json格式，否则为每行一条记录的方式。
       - `-deps` 还会列出它们的依赖。
       - `-u` 可以列出可升级的库的信息，如`go list -m -u -json all`。
       - `-versions` 显示库的可用的版本列表。 

- **all**
    1. **概念**
       - `ll`现在是`go action`中一个特殊的语义，代表`main module`和它的依赖。
       - `...`代表代表匹配这个模式的所有活动的module
       - `...`代表匹配任何字符串，包括空格和斜杠，但是不会匹配vendor文件夹。
       - `go get all`、`go list all`、`go get -u all`、`go get ./...`、`go get -u ./...`都是合法的。

    2. **go特殊保留字**
       - `main`: 可执行程序的顶层包
       - `all`: `GOPATH`所有的包; `module`模式下指`main module`和它的依赖。
       - `std`: 标准库的包
       - `cmd`: go仓库里的命令行工具和它们的内部库






