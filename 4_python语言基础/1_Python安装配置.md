## Mac系统安装Python

### 1.安装方式

- 直接从官网下载python安装包，根据提示安装即可。

- Homebrew安装python

  1. 先安装xcode和command line tools

     ```shell
     # 终端输入
     xcode-select –p
     # 若显示以下内容,则已安装过
     /Applications/Xcode.app/Contents/Developer
     # 否则终端输入以下命令安装
     xcode-select –install
     ```

  2. 安装Homebrew

     ```shell
     # 安装Homebrew
     /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
     # 验证安装Homerbrew
     brew doctor
     ```

  3. 安装python

     ```shell
     # 搜索python
     brew search python
     # 安装python
     brew install python3
     # 验证安装
     python3 –version
     ```

### 2.Python安装位置

- **Mac自带的Python**

  ```shell
  # python环境位置
  /System/Library/Frameworks/Python.framework/Versions/2.7
  # 解释器
  ./bin/python2.7
  # 默认启动路径
  /usr/bin
  ```

- **手动安装的Python**

  ```shell
  # python环境位置
  /Library/Frameworks/Python.framework/Versions/3.4
  # 解释器
  ./bin/python3.4
  
  # 用户安装Anaconda3后，自带的python环境
  /Users/steven/Anaconda3 (anaconda在安装时候的自定义路径)
  # 解释器
   ./bin/python3.4
   
  # 默认启动路径
  /usr/local/bin
  ```

- **Homebrew安装的Python**

  ```shell
  # python环境位置
  /usr/local/Cellar/python@3.8
  # 默认启动路径
  暂无
  ```



### 3.系统环境变量配置

- 配置python命令调用的版本

  ```shell
  # 修改文件 ~/.zshrc 或者 ~/.bash_profile
  export PATH=$PATH:/usr/local/bin/python==
  ```

  