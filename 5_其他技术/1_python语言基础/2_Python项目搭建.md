## 基本工具

### 1.包管理工具pip

- **作用**

  1. PIP是通用的Python包管理工具，提供了对 Python 包的查找、下载、安装、卸载、更新等功能。安装诸如Pygame、Pymysql、requests、Django等Python包时，都要用到pip。
  2. 在Python3.4或者Python2.7及更新的版本中，PIP已经捆绑安装了，不需要再单独安装，但是还需要更新。
  3. PIP命令安装包的位置（pip命令模块本身也位于该位置）：`/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages`

- **更新pip版本**

  ```shell
  # 查看pip版本
  pip --version
  # 使用pip命令更新
  pip install --upgrade pip
  # 使用python命令更新
  python -m pip install --upgrade pip
  ```

- **设置pip源**

  ```shell
  # 源列表
  阿里云 mirrors.aliyun.com/pypi/simple…
  中国科技大学 pypi.mirrors.ustc.edu.cn/simple/
  豆瓣 pypi.douban.com/simple/
  清华大学 pypi.tuna.tsinghua.edu.cn/simple/
  中国科学技术大学 pypi.mirrors.ustc.edu.cn/simple/
  华中理工大学 pypi.hustunique.com/
  山东理工大学 pypi.sdutlinux.org/
  v2ex pypi.v2ex.com/simple/
  
  # 显示当前的pip源
  pip show 
  # 临时修改
  pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 安装包名字
  # 永久修改
  pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
  # 修改配置文件mac 和 Linux  ~/.pip/pip.conf(如果没有则创建)
  [global]
  index-url = https://pypi.tuna.tsinghua.edu.cn/simple
  ```

- **pip导出python包**

  ```shell
  # 一键导出包配置
  pip freeze > requirement.txt
  # 安装所有包配置
  pip install -r requirement.txt
  ```

- **常用pip命令**

  ```shell
  # 查看安装的模块和版本名
  pip list
  # 安装模块
  pip install 模块名
  # 安装指定版本
  pip install 模块名==版本号
  # 卸载模块
  pip uninstall 模块名
  ```

  

### 2.虚拟环境配置工具

#### 2.1virtualenv

- **作用**

  1. `virtualenv` 是目前最流行的 Python 虚拟环境配置工具。它不仅同时支持 Python2 和 Python3，而且可以
     为每个虚拟环境指定 Python 解释器，并选择不继承基础版本的包。
  2. 使得不同Python应用的开发环境相互独立，可以防止系统中出现包管理混乱和版本的冲突。
  3. 开发环境升级不影响其他应用的开发环境，也不会影响全局的环境（默认开发环境是全局开发环境）,因为虚拟环境是将全局环境进行私有的复制，当我在虚拟环境进行 pip install 时，只会安装到选择的虚拟环境中。

- **基本使用**

  ```shell
  # 1.pip安装virtualenv
  pip install virtualenv 
  
  # 2.创建虚拟环境目录
  mkdir myproject
  cd myproject
  
  # 3.创建一个独立的Python运行环境，--no-site-packages其意义在于不复制已经安装到系统Python环境中的所有第三方包从而得到一个“纯净”的运行环境。
  virtualenv --no-site-packages myenv  
  
  # 4.激活虚拟运行环境
  # Windows：
  myenv\Scripts\activate.bat
  # Linux:
  source myenv/bin/activate
  
  # 5.安装各种第三方包，并运行Python命令
  pip install jieba
  python myapp.py
  
  # 6.使用deactivate命令退出当前的myenv环境
  deactivate
  ```

  

#### 2.2venv

- **作用**

  1. Python 从3.3 版本开始，自带了一个虚拟环境 venv，在 PEP-405 中可以看到它的详细介绍。
  2. 很多操作都和 virtualenv 类似，但是两者运行机制不同。因为是从 3.3 版本开始自带的，这个工具也仅仅支持 python 3.3 和以后版本。所以，要在 python2 上使用虚拟环境，依然要利用 virtualenv 。

- **基本使用**

  ```shell
  # 1.安装
  # Windows 中venv已经以标准库的形式存在，不用再单独安装
  # Linux
  sudo apt-get install python3-venv  
  
  # 2.在当前目录创建一个独立的Python运行环境
  # Windows
  py -3 -m venv myenv  
  # Linux 
  python3 -m venv myenv
  ```

  

#### 2.3pipenv

- **作用**
  1. pipenv 是 Pipfile 主要倡导者、requests 作者 Kenneth Reitz 写的一个命令行工具，主要包含了Pipfile、pip、click、requests和virtualenv，能够有效管理Python多个环境，各种第三方包及模块。
  2. pipenv集成了pip，virtualenv两者的功能，且完善了两者的一些缺陷。
  3. 过去用virtualenv管理requirements.txt文件可能会有问题，Pipenv使用Pipfile和Pipfile.lock，后者存放将包的依赖关系，查看依赖关系是十分方便。
  4. 各个地方使用了哈希校验，无论安装还是卸载包都十分安全，且会自动公开安全漏洞。
  5. 通过加载.env文件简化开发工作流程。
  6. 支持Python2 和 Python3，在各个平台的命令都是一样的。





### 3.Django-Web项目搭建

- **安装Django**

  ```shell
  pip install Django==1.6.11
  ```

- **创建项目**

  ```shell
  django-admin.py startproject my_blog .
  ```

- **创建数据库（一般使用mysql）**

  ```shell
  # 运行命令后，工程目录下就会多了一个 db.sqlite3 文件
  python manage.py migrate
  ```

- **启动项目**

  ```shell
  # 访问http://127.0.0.1:8000/  ，ctrl+c即可关闭
  python manage.py runserver
  ```

  

