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



### 3.protoc和protoc-gen-go产生的代码的区别

- **命令来源/出处**

  1. protoc 命令来自于 https://github.com/google/protobuf，可以产生序列化和反序列化的代码，无go相关代码。
  2.  protoc-gen-go插件则来自于[https://github.com/golang/protob](https://github.com/golang/protobuf/)uf/protoc-gen-go， 可以产生go相关代码， 除上述序列化和反序列化代码之外， 还增加了一些通信公共库。

  ```shell
  # protoc编译方法
  protoc --go_out=./go1/ ./proto/my.proto
  
  # protoc-gen-go编译方法
  protoc --go_out=plugins=grpc:./go2/  ./proto/my.proto
  ```

- **生成go代码的不同原因**

  1. protobuf版本，所影响的只有proto文件的语法，具体可参照[https://colobu.com/2019/10/03/protobuf-ultimate-tutorial-in-go/#%E7%BC%96%E7%A0%81](https://colobu.com/2019/10/03/protobuf-ultimate-tutorial-in-go/#编码)
  2. proto-gen-go的版本，所影响的是生成go语言的版本，1.4和1.4之前是两套版本。



### 4.go代码生成

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

  



