[TOC]

**文件I/O常用头文件**

```c++
#include <sys/types.h>    //定义数据类型，如ssize_t等

#include <fcntl.h>        //定义open,create等函数原型，创建文件权限的符号常量S_IRUSR等

#include <unistd.h>       //定义read,write,close,lseek等函数原型

#include <errno.h>        //与全局变量errno相关的定义

#include <sys/ioctl.h>    //定义ioctl函数原型
```

**目录**

[一、open函数](#t0)

[二、creat函数](#t1)

[三、close函数](#t2)

[四、read函数](#t3)

[五、write函数](#t4)

[六、fsync函数](#t5)

[七、lseek函数](#t6)

[八、ioctl函数](#t7)

------

### 一、open函数

**1.进行文件I/O操作时，要先调用open()函数打开文件，它返回的文件描述符fd就代表了所打开的文件，后续的其他操作均通过引用该文件描述符fd进行。open()函数原型在<fcntl.h>中定义：**

```
int open(const char *pathname, int flags, .../*mode_t mode*/);
```

**2.1 open()参数**

![image-20210711153842904](C:\Users\xzl\AppData\Roaming\Typora\typora-user-images\image-20210711153842904.png)

**2.2 mode参数可用的符号常量表**

| **符号常量** | **值** |         **含义**         | **符号常量** | **值** |        **含义**        |
| :----------: | :----: | :----------------------: | :----------: | :----: | :--------------------: |
|   S_IRWXU    | 0x700  | 所属用户读、写和执行权限 |   S_IRWXG    | 0x070  | 组用户读、写和执行权限 |
|   S_IRUSR    | 0x400  |      所属用户读权限      |   S_IRGRP    | 0x040  |      组用户读权限      |
|   S_IWUSR    | 0x200  |      所属用户写权限      |   S_IWGRP    | 0x020  |      组用户写权限      |
|   S_IXUSR    | 0x100  |     所属用户执行权限     |   S_IXGRP    | 0x010  |     组用户执行权限     |
|   S_IRWXO    | 0x007  | 其他用户读、写和执行权限 |   S_IWOTH    | 0x002  |     其他用户写权限     |
|   S_IROTH    | 0x004  |      其他用户读权限      |   S_IXOTH    | 0x001  |    其他用户执行权限    |

### 二、creat函数

**1.open()函数的参数flags，当设置了O_CREAT标志时，可以创建一个新文件，也可以用另外一个函数creat()创建新文件，creat()函数原型在<fcntl.h>文件中定义：**

```
int creat(const char *pathname, mode_t mode);    //参数pathname,mode同open
```

**2.open与creat区别注意：**

**（1）creat()创建文件时，\**\*如果文件已存在，则会把已存在的文件内容清空、长度截为0\*\**，然后返回对应的文件描述符；如果文件不存在，则直接创建，然后返回文件描述符**

  **(2)  当open()的参数flags设置了O_CREAT时，如果文件已存在，则直接打开并返回文件描述符；\**\*如果文件不存在，则创建新文件\*\**，然后返回对应的文件描述符**

 

### **三、close函数**

**1.文件I/O操作完成后，应该调用close()关闭打开的文件，释放打开文件时所占用的系统资源，close()函数原型在<unistd.h>中定义：**

```
int close(int fd);    //fd为open()或creat()返回的文件描述符
```

### 四、read函数

**1.从打开的文件读取数据，可调用read()函数实现，read()函数原型在<unistd.h>中定义：**

```Swift
ssize_t read(int fd, void *buf, size_t count);
```

**操作成功，返回实际读取的字符数，如果已达到文件结尾，返回0,，否则返回-1**

**2.参数：**

**（1）fd：open()或creat()返回的文件描述符**

**（2）buf：用来接收所读数据的缓冲区**

**（3）count：请求读取的字节数，其中若文件长度小于请求的长度，则首次调用read时，返回文件长度，下次调用时返回0。读设备文件时，有些设备每读到一行数据，就返回**

 

### 五、write函数

**1.把数据写入文件，可调用write()函数实现，write()函数原型在<unistd.h>中定义：**

```
ssize_t write(int fd, void *buf, size_t count);
```

**操作成功，返回实际写入的字节数，出错则返回-1**

**2.参数：同read()**

 

### **六、fsync函数**

**1.write()函数一旦返回，表明所写的数据已提交到系统内部缓存，但此时数据不一定写入了磁盘等持久存储设备中。要确保已修改过的数据全部写入持久设备中，正确的做法是调用fsync函数进行文件数据同步，强制把已修改的文件数据写入持久存储设备中。其原型在<unistd.h>中定义：**

```
int fsync(int fd);
```

**操作成功返回0，否则返回-1**

### 七、lseek函数

**1.Linux文件按读写方式可分为顺序读写和随机读写文件。普通磁盘文件一般都能随机读写，这类文件可通过lseek()函数改变文件读写位置。lseek()函数不会读写任何文件数据，仅仅是改变文件的起始读写位置。lseek()函数原型在<unistd.h>中定义：**

```
off_t lseek(int fd, off_t offset, int whence);
```

**如果操作成功，lseek()返回新的读写位置；否则返回-1**

**2.参数：**

**（1）offset：目标位置**

**（2）whence：偏移的参照点，有效值是SEEK_SET、SEEK_CUR、SEEK_END，含义如下：**

​     **SEEK_SET：从文件开头算起**

​     **SEEK_CUR：从当前所在的位置算起，偏移offset字节，正值表示往文件尾部偏移，负值表示往文件头部偏移**

​     **SEEK_END：从文件结尾算起，正值表示往文件尾部偏移，负值表示往文件头部偏移**

### **八、ioctl函数**

**1.文件I/O操作还有很多不好归到read()/write()的，只好放在这个函数中。很多设备文件用过ioctl()函数提供设备特有的操作，比如修改设备寄存器的值。ioctl()是文件i/o的杂项函数，其函数原型在<sys/ioctl.h>中定义：**

```
int ioctl(int fd, int cmd, ...);
```

**操作成功返回0；失败返回-1**

**2.参数**

**（1）cmd：文件的操作命令。该命令是文件专有的，不同的文件，cmd往往是不同的**

**（2）“...”：表示从参数是可选的、类型不确定的**

------

**示例代码：**

```c++
#include <stdio.h>
#include <fcntl.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/ioctl.h>

int main(int argc, char *argvp[])
{
	char buf[128] = { 0 };
	char w_str[] = "Hello,Linux file.\n";
	char filename[] = "hello.txt";
	int fd, rec;
	int mode = 0x664;
	int cur, newcur, offset = 0;
	fd = open(filename, O_RDWR|O_CREAT, mode);	//以可读写、创建标志打开文件,mode=0x664
	if (fd < 0)
	{
		printf("open file error.\n");
		return -1;
	}
	rec = read(fd, buf, sizeof(buf));	//读文件
	if (rec < 0)
		printf("read file error.\n");
	rec = write(fd, w_str, sizeof(w_str));	//写文件
	if (rec < 0)
		printf("write file error.\n");
	cur = lseek(fd, 0, SEEK_CUR);	//获取当前读写位置
	offset = 18 - cur;	//移动到偏移18个字节的位置读数据
	newcur = lseek(fd, offset, SEEK_CUR);	//从当前位置,移动到18
	if (newcur == -1)
		printf("lseek error.\n");
	fsync(fd);	//同步文件
	close(fd);	//关闭文件
	return 0;

}
```

 

