## 环境搭建

### 1.protobuf安装

- mac使用brew安装

  ```shell
  # 搜索资源
  brew search protobuf
  # 安装最新版本
  brew install protobuf 
  # 安装指定版本
  brew install protobuf@f3.6
  # 重装
  brew reinstall protobuf
  # 卸载
  brew uninstall protobuf
  ```

- 使用源文件编译安装

  ```shell
  # github地址，https://github.com/protocolbuffers/protobuf/
  # 待整理
  ```

  

### 2.protoc-gen-go插件安装

- protobuf使用插件原理
  1. protoc-gen-go是一个go项目，go install编译安装后，会生成一个可执行文件在bin目录下。
  2. protoc执行命令的时候，就会到gopath/bin目录下，拿到执行文件。
  3. protoc-gen-go的版本，1.4和1.4之前是两套版本，需要注意。

### 3.go代码生成

- 执行脚本

  ```shell
  #!/bin/bash
  
  if [ $# -eq 0 ] ;
  then
  	echo "Usage: $0 demo.proto"
  	exit 1
  fi
  
  fileName=`basename $1`
  baseDir=`basename $fileName .proto`
  targetDir="protobuf3.pb/$baseDir/"
  
  if [ ! -d $targetDir ];
  then
  	mkdir $targetDir
  fi
  # --go_out=plugins=grpc 是使用了protoc-gen-go插件，用来生成go代码
  cmd="protoc --go_out=plugins=grpc,paths=source_relative:$targetDir -I=${GOPATH}/src -I=. $1"
  echo $cmd
  eval $cmd
  ```

  



