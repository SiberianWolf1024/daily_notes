# **CentOS 7.6下使用CMake构建C++项目学习文档**

CMake是一个跨平台的开源构建工具，它可以自动生成用于不同构建系统（如Makefile、Ninja等）的配置文件，从而简化了项目的构建过程。

## 1.安装CMake和C++编译器

通过以下命令来安装CMake和C++编译器：

	sudo yum update # 更新软件包信息
	sudo yum install cmake # 安装CMake
	sudo yum install gcc-c++ # 安装C++编译器

安装完成后，可以通过以下命令检查是否安装成功：

	cmake --version
	g++ --version

## 2.安装Visual Studio Code

第一步：导入Microsoft GPG密钥：

```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
```

第二步：创建以下存储库文件以启用Visual Studio代码存储库：

```
sudo vim /etc/yum.repos.d/vscode.repo
```

将以下内容粘贴到文件中： /etc/yum.repos .d / vscode.repo

```
[code]
name=Visual Studio Code
baseurl=https://packages.microsoft.com/yumrepos/vscode
enabled=1
gpgcheck=1
gpgkey=https://packages.microsoft.com/keys/microsoft.asc
```

保存文件并关闭文本编辑器。

存储库已启用，键入以下内容以安装最新版本的Visual Studio代码：

```
sudo yum install code
```

第三步：配置环境变量
安装完成后的vscode 是不能直接以root身份打开的，需要设置一些参数，但是对于每次打开而言很麻烦，所以将其配置进入环境变量中，方便每次打开。

\# root 用户

```
vim ~/.bashrc
```

\# 添加一行

```
alias code='/usr/share/code/code . --no-sandbox --unity-launch'
```

 \# 保存后，执行下边的命令生效

```
source ~/.bashrc 
```

打开vscode：切换至root用户，输入命令code

```
su

code
```

安装以下四个扩展插件：

C/C++、C/C++Extension Pack、CMake、CMake Tools

## 3.代码编译

在项目文件夹中，创建一个CMakeLists.txt文件，用于配置CMake项目。示例CMakeLists.txt内容如下：

#设置Cmake最低版本要求

```
cmake_minimum_required(VERSION 3.0)
```

#定义工程名称

```
project(MyProject)
```

#设置C++编译器参数

```
set(CMAKE_CXX_STANDARD 11)
```

#生成可执行文件

```
add_executable(my_program main.cpp)
```

## 4.编写C++源代码：

在项目文件夹中，创建一个名为main.cpp的文件，并编写一些C++代码。例如：

```
#include <iostream>

using namespacestd;

int main() {
  cout << "Hello, CMake!" << endl;
  return 0;
}
```

## 5.构建项目：

在VSCode中打开项目文件夹，按下`Ctrl + Shift + B`或者点击顶部菜单中的“终端(Terminal)” -> “运行生成任务(Run Build Task)”来构建项目。选择`cmake`构建选项，CMake将会生成构建系统所需的Makefile。

## 6.运行项目：

通过终端运行生成的可执行文件，在VSCode终端中输入以下命令来运行项目：

```
./my_executable
```



### CentOS 7使用vscode下载插件时出现 **vscode “提取扩展时出错，XHR failed”**

解决方法：

按F1键，输入  Developer: Toggle Developer Tools回车，接着搜索插件，注意弹出的错误信息

![vevev](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202310262137648.png)

说明vscode连不上 marketplace.visualstudio.com/_apis/public/gallery/extensionquery:1

先ping一下这个网址，能ping成功则可进行下一步

```c++
ping marketplace.visualstudio.com
```

然后打开查询ip的网站https://ip.tool.chinaz.com/查询，记住ip

![image-20231026213839323](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202310262138350.png)

打开终端，输入sudo nano /etc/hosts命令进入hosts文件，在任意一行写上 **刚才的查询的ip地址+空格+连接不上的域名**，如我是13.107.42.18 marketplace.visualstudio.com，然后Ctrl+O再回车保存，Ctrl+X退出编辑，重启vscode即可

```c++
sudo nano /etc/hosts
// 任意一行加上13.107.42.18 marketplace.visualstudio.com
```



文件（文件夹）复制命令

```c++
// 将 ~/test/ 目录下的 文件file1 复制到 ~/bin/shell/ 目录下
cp test/file1 bin/shell
// 将 ~/test/ 目录下的 file1 复制到 ~/bin/shell/ 目录下，并将文件名改为 file2
cp test/file1 bin/shell/file2
// 将 ~/test/ 目录下的 文件夹file1 复制到 ~/bin/shell/ 目录下
cp -a test/file1 bin/shell
```

删除文件夹

```c++
# 删除 ~/test/ 目录下的 01 文件夹（如果当前已经是 test 目录，直接 rm -rf 01 即可）
rm -rf  ~/test/01
```

压缩命令：

```c++
tar -zcvf 压缩文件名 .tar.gz 被压缩文件名
```

解压缩命令：

```c++
tar -zxvf 压缩文件名.tar.gz
```

查看文件夹大小：

```c++
// 查看整个文件夹大小
du -sh /path/to/directory
// 查看文件夹及其中各子目录大小
du -h /path/to/directory
```

查看某个目录的磁盘空间：

```c++
df -h /path/to/directory
```

ppp-rtk-development编译时一直报错找不到动态库

```c++
cmake  -DCMAKE_BUILD_TYPE=Release .. 
```

```c++
https://zhuanlan.zhihu.com/p/373256365
```

