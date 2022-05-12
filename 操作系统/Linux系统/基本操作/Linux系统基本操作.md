[TOC]

### Linux系统目录：

```c++
bin：存放二进制可执行文件

boot：存放开机启动程序

dev：存放设备文件： 字符设备、块设备

home：存放普通用户

etc：用户信息和系统配置文件 passwd、group

lib：库文件：libc.so.6

root：管理员宿主目录（家目录）

usr：用户资源管理目录
```

### Linux系统文件类型： 7/8 种

	普通文件：-
	
	目录文件：d
	
	字符设备文件：c
	
	块设备文件：b
	
	软连接：l
	
	管道文件：p
	
	套接字：s

### linux命令操作

```c++
ls --help ;           ls -a;            ls -l;      

cd -;   切换到上一次目录  

df：  查看磁盘内存；

mkdir ;               touch;

echo;                 cat;              tree;

软连接：    ln -s  绝对路径 软连接名；

硬链接：    ln  绝对路径 硬链接名；

修改权限：  chmod ;(4+2+1)

创建用户：  sudo adduser 新用户名；

修改用户密码：  sudo passwd 用户名；

查找：     find   查找目录   -name   ‘文件名 ’；

grep  搜索文件；      配合管道       |  grep   '搜索内容'

源更新到本地： sudo apt-get update

安装软件：    sudo apt-get install 要安装软件的名字；

卸载软件：    sudo apt-get remove  软件名；

deb包安装:   sudo dpkg -i xxx.deb;

进程管理：   
ps  aux               查看系统进程； 
kill  进程ID           杀死进程；
env                   查看环境变量；
top                   查看内存和CPU；
kill -l               查看信号量64个；

网络管理：  ifconfig   ping   netstate
```

### vim

三种模式：文本模式、命令模式、末行模式

![vim模式](E:\xzl\课程学习\黑马程序员Linux系统编程\linux编程及ubuntu基础资料\linux系统编程资料\day2\2-其他资料\工作模式.png)

```
i 当前光标位置；I 当前光标行首；

o当前光标下一行；O当前光标上一行；

a当前光标位置；A当前光标行尾；
```

### gcc编译

![gcc编译4步骤](E:\xzl\课程学习\黑马程序员Linux系统编程\linux编程及ubuntu基础资料\linux系统编程资料\day2\2-其他资料\gcc编译4步骤.png)

gcc 要编译的文件名 -o  生成的可执行名





