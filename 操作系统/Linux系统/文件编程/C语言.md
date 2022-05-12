[TOC]

# 文件操作

## 1. 文件概述

### 1.1 磁盘文件

指一组相关数据的有序集合,通常存储在外部介质(如磁盘)上，使用时才调入内存;

#### 磁盘文件的分类:

从用户或者操作系统使用的角度（逻辑上）把文件分为:

- 文本文件：基于字符编码的文件
  - 基于字符编码，常见编码有ASCII、UNICODE等;
  - 一般可以使用文本编辑器直接打开;
  - 数5678的以ASCII存储形式(ASCII码)为：
    00110101 00110110 00110111 00111000
- 二进制文件：基于值编码的文件
  - 基于值编码,自己根据具体应用,指定某个值是什么意思;
  - 把内存中的数据按其在内存中的存储形式原样输出到磁盘上
  - 数5678的存储形式(二进制码)为：00010110 00101110

### 1.2 设备文件

在操作系统中把每一个**与主机相连的输入、输出设备看作是一个文件**，把它们的输入、输出等同于对磁盘文件的读和写

- stdin(标准输入文件)
- stdout(标准输出文件)
- stderr(标准错误文件)

> 当程序启动时,系统默认打开这三个文件;

------

## 2. 文件指针

C语言中**用一个指针变量指向一个文件**，这个指针称为文件指针

```c
typedef struct
{
	short           level;	//缓冲区"满"或者"空"的程度 
	unsigned        flags;	//文件状态标志 
	char            fd;		//文件描述符
	unsigned char   hold;	//如无缓冲区不读取字符
	short           bsize;	//缓冲区的大小
	unsigned char   *buffer;//数据缓冲区的位置 
	unsigned        ar;	 //指针，当前的指向 
	unsigned        istemp;	//临时文件，指示器
	short           token;	//用于有效性的检查 
}FILE;
123456789101112
```

> FILE是系统使用typedef定义出来的有关文件信息的一种结构体类型，结构中含有文件名、文件状态和文件当前位置等信息;

> 声明FILE结构体类型的信息包含在头文件“stdio.h”中，一般设置一个指向FILE类型变量的指针变量，然后通过它来引用这些FILE类型变量。通过文件指针就可对它所指的文件进行各种操作

C语言中有三个特殊的文件指针**由系统默认打开**，用户无需定义即可直接使用

- **stdin**： 标准输入，默认为当前终端（键盘），我们使用的scanf、getchar函数默认从此终端获得数据;
- **stdout**：标准输出，默认为当前终端（屏幕），我们使用的printf、puts函数默认输出信息到此终端;
- **stderr**：标准出错，默认为当前终端（屏幕），我们使用的perror函数默认输出信息到此终端

------

## 3.操作文件

### 3.1 打开文件:`fopen`

任何文件使用之前必须打开：

```c
#include <stdio.h>
FILE * fopen(const char * filename, const char * mode);
功能：打开文件
参数：
	filename：需要打开的文件名，根据需要加上路径
	mode：打开文件的模式设置
返回值：
	成功：文件指针
	失败：NULL
	
参数mode的几种形式:
r或rb	以只读方式打开一个文本文件（不创建文件，若文件不存在则报错）
w或wb	以写方式打开文件(如果文件存在则清空文件，文件不存在则创建一个文件)
a或ab	以追加方式打开文件，在末尾添加内容，若文件不存在则创建文件
r+或rb+	以可读、可写的方式打开文件(不创建新文件)
w+或wb+	以可读、可写的方式打开文件(如果文件存在则清空文件，文件不存在则创建一个文件)
a+或ab+	以添加方式打开文件，打开文件并在末尾更改文件,若文件不存在则创建文件
1234567891011121314151617
```

##### 注意:

1. b是二进制模式的意思，b只是在Windows有效，在Linux用r和rb的结果是一样的;
2. Unix和Linux下所有的文本文件行都是\n结尾，而Windows所有的文本文件行都是\r\n结尾
3. 在Windows平台下，以“文本”方式打开文件，不加b：
   - 当读取文件的时候，系统会将所有的 “\r\n” 转换成 “\n”
   - 当写入文件的时候，系统会将 “\n” 转换成 “\r\n” 写入;
   - **以"二进制"方式打开文件，则读写都不会进行这样的转换**
4. 在Unix/Linux平台下，“文本”与“二进制”模式没有区别，"\r\n" 作为两个字符原样输入输出;

### 3.2 关闭文件:`fclose`

任何文件在使用后应该关闭：

```
#include <stdio.h>
int fclose(FILE * stream);
功能：关闭先前fopen()打开的文件。此动作让缓冲区的数据写入文件中，并释放系统所提供的文件资源。
参数：
	stream：文件指针
返回值：
	成功：0
	失败：-1
12345678
```

##### 注意:

1. 打开的文件会占用内存资源，如果总是打开不关闭，会消耗很多内存;
2. 一个进程**同时打开的文件数是有限制的**，超过最大同时打开文件数，再次调用fopen打开文件会失败;
3. 如果没有明确的调用fclose关闭打开的文件，那么程序在退出的时候，操作系统会统一关闭

##### 示例:

```c
#include <stdio.h>

void openFile() {
    FILE *file = fopen("./test.txt", "r+");
    FILE *file1 = NULL;
    if (NULL == file) {
        file1 = fopen("./test.txt", "w+");
    } else {
        fputs("hello file", file);
        fclose(file);
    }
    if (file1 != NULL) {
        printf("start writing:\n");
        fputs("hahaha", file1);
        fclose(file1);
    }
}

int main() {
    openFile();
    return 0;
}
12345678910111213141516171819202122
```

##### 路径问题:

1. 在cLion中,编译生成的exe文件都存放在`cmake-build-debug`目录下,因此相对路径是该exe文件所在的这个目录下面,而不是工程文件;

### 3.3 文件的顺序读写:`fputc fgetc`

#### 3.3.1 按照字符读写文件

#### (1) 写文件:

```c++
#include <stdio.h>
int fputc(int ch, FILE * stream);
功能：将ch转换为unsigned char后写入stream指定的文件中
参数：
	ch：需要写入文件的字符
	stream：文件指针
返回值：
	成功：成功写入文件的字符
	失败：返回-1
123456789
```

##### 判断文件结尾

1. 使用EOF(end of file)来判断文件结尾,但是这种方式只能判断文本文件的结尾;

2. feof函数

   (既可用以判断二进制文件又可用以判断文本文件)

   ```c
   #include <stdio.h>
   int feof(FILE * stream);
   功能：检测是否读取到了文件结尾。判断的是最后一次“读操作的内容”，不是当前位置内容(上一个内容)。
   参数：
   	stream：文件指针
   返回值：
   	非0值：已经到文件结尾
   	0：没有到文件结尾
   12345678
   ```

#### (2) 读取文件

```c
#include <stdio.h>
int fgetc(FILE * stream);
功能：从stream指定的文件中读取一个字符
参数：
	stream：文件指针
返回值：
	成功：返回读取到的字符
	失败：-1
12345678
```

##### 示例:

```c
#include <stdio.h>


void test_fputc() {
    FILE *file = fopen("./test.txt", "r+");
    if (file == NULL) {
        file = fopen("./test.txt", "w+");
    }
    fputc('d', file);
    fclose(file);
}

void test_fgetc() {
    FILE *file = fopen("./test.txt", "r+");
    if (file == NULL) {
        file = fopen("./test.txt", "w+");
    }
    int i = fgetc(file);
    printf("%c", i); //d
}
//循环写入和读取
void test_read_write() {
    FILE *file = fopen("./test.txt", "w+");
    char buf[] = "what are you doing?";
    int i = 0;
    while (buf[i] != 0) {
        fputc(buf[i], file);
        i++;
    }
    fclose(file);
    FILE *file1 = fopen("./test.txt", "r");
    char str[32] = "";
    int j = 0;
    while ((str[j] = fgetc(file1)) != -1) {
        j++;
    }
    printf("str=%s", str);
}

int main() {
    test_fputc();
    test_fgetc();
}
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

#### 实现文件的copy:

```c
// STATEMENT : 文件copy案例

#include <stdio.h>

void copy_file(FILE *file, FILE *file1);

int main() {
    FILE *file = fopen("./test.txt", "r");
    FILE *file1 = fopen("./backup.txt", "w+");
    copy_file(file, file1);
    fclose(file);
    fclose(file1);
    return 0;
}

void copy_file(FILE *file, FILE *file1) {
    if (NULL == file) {
        printf("源文件不存在!\n");
        return;
    }
    while (feof(file) == 0) {
        char ch = fgetc(file);
        fputc(ch, file1);
    }
}
12345678910111213141516171819202122232425
```

#### 3.3.2 按照行读写文件:`fgets fputs`

#### (1) 读文件

```c++
#include <stdio.h>
char * fgets(char * str, int size, FILE * stream);
功能：从stream指定的文件内读入字符，保存到str所指定的内存空间，直到出现换行字符、读到文件结尾或是已读了size - 1个字符为止，最后会自动加上字符 '\0' 作为字符串结束。
参数：
	str：字符串
	size：指定最大读取字符串的长度（size - 1）
	stream：文件指针
返回值：
	成功：成功读取的字符串
	读到文件尾或出错： NULL
    
fgets若没遇到换行符，会接着从前一次的位置继续读入n-1个字符,只要是文本流没关闭。
```

#### (2) 写文件

```c++
#include <stdio.h>
int fputs(const char * str, FILE * stream);
功能：将str所指定的字符串写入到stream指定的文件中，字符串结束符 '\0'  不写入文件。 
参数：
	str：字符串
	stream：文件指针
返回值：
	成功：0
	失败：-1
123456789
```

#### 示例:

```c
// STATEMENT : fputs和fgets

#include <stdio.h>

void write_string() {
    FILE *file = fopen("./test.txt", "a");
    char buf[] = "hello";
    fputs(buf, file);
    fputs(buf, stdout);
    fclose(file);
}

void read_string() {
    FILE *file = fopen("./test.txt", "r");
    char str[16] = "";
    while (1) {
        char *string = fgets(str, sizeof(str), file);
        if (string == NULL) {
            break;
        }
        printf("%s\n", string);
    }
}


int main() {
    /* write_string();*/
    read_string();
    return 0;
}
123456789101112131415161718192021222324252627282930
```

#### 3.3.3 按照格式化文件:`fscanf fprintf`

#### (1) 读文件

```c++
#include <stdio.h>
int fscanf(FILE * stream, const char * format, ...);
功能：从stream指定的文件读取字符串，并根据参数format字符串来转换并格式化数据。
参数：
	stream：已经打开的文件
	format：字符串格式，用法和scanf()一样
返回值：
	成功：参数数目，成功转换的值的个数
	失败： - 1
123456789
```

#### (2) 写文件

```c++
#include <stdio.h>
int fprintf(FILE * stream, const char * format, ...);
功能：根据参数format字符串来转换并格式化数据，然后将结果输出到stream指定的文件中，指定出现字符串结束符 '\0'  为止。
参数：
	stream：已经打开的文件
	format：字符串格式，用法和printf()一样
返回值：
	成功：实际写入文件的字符个数
	失败：-1
123456789
```

##### 示例:

```c
#include <stdio.h>

void test_fprintf() {
    int year = 2018;
    int month = 10;
    int day = 27;
    char buf[1024] = "";
    FILE *fp = NULL;
    fp = fopen("./fprintf.txt", "w");
    if (!fp) {
        perror("");
        return;
    }
    //sprintf(buf,"%04d:%02d:%02d\n",year,month,day);
    //fputs(buf,fp);
    fprintf(fp, "%04d:%02d:%02d\n", year, month, day);
    fclose(fp);
}

void test_fscanf() {
    int year = 0, month = 0, day = 0;
    FILE *fp = NULL;
    fp = fopen("./fprintf.txt", "r");
    if (!fp) {
        perror("");
        return ;
    }
    fscanf(fp, "%d:%d:%d\n", &year, &month, &day);
    printf("%d  %d %d \n", year, month, day);
}
123456789101112131415161718192021222324252627282930
```

#### 3.3.4 按照块读写文件:`fread fwrite`

> 一般用于处理大文件

#### (1) 读文件

```c++
#include <stdio.h>
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
功能：以数据块的方式从文件中读取内容
参数：
	ptr：存放读取出来数据的内存空间
	size： size_t 为 unsigned int类型，此参数指定读取文件内容的块数据大小
	nmemb：读取文件的块数，读取文件数据总大小为：size * nmemb
	stream：已经打开的文件指针
返回值：
	成功：实际成功读取到内容的块数，如果此值比nmemb小，但大于0，说明读到文件的结尾。
	失败：0
1234567891011
```

#### (2) 写文件

```c++
#include <stdio.h>
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
功能：以数据块的方式给文件写入内容
参数：
	ptr：准备写入文件数据的地址
	size： size_t 为 unsigned int类型，此参数指定写入文件内容的块数据大小
	nmemb：写入文件的块数，写入文件数据总大小为：size * nmemb
	stream：已经打开的文件指针
返回值：
	成功：实际成功写入文件数据的块数目，此值和 nmemb 相等
	失败：0
1234567891011
```

##### 示例:

```c
#include <stdio.h>

typedef struct {
    char name[16];
    int id;
    double salary;
} man;

void test_fwrite() {
    man m[3] = {
            {"lisa", 1, 5599},
            {"Tom",  2, 9999},
            {"Sam",  3, 12999}
    };
    FILE *file = fopen("./exerFiles/fwrite.txt", "w");
    if (NULL == file) {
        perror("");
        return;
    }
    size_t i = fwrite(m, 1, sizeof(m), file);
    if (i != 0) {
        printf("success\n");
    }
    fclose(file);
}

void test_fread() {
    FILE *file = fopen("./exerFiles/fwrite.txt", "r");
    if (NULL == file) {
        perror("");
        return;
    }
    man m[3];
    for (int i = 0; i < 3; i++) {
        fread(&m[i], 1, sizeof(m[0]), file);
    }
    //遍历查看
    for (int i = 0; i < sizeof(m) / sizeof(m[0]); i++) {
        printf("%s %d %f\n", m[i].name, m[i].id, m[i].salary);
    }
    //lisa 1 5599.000000
    //Tom 2 9999.000000
    //Sam 3 12999.000000
}

int main() {
    test_fwrite();
    test_fread();
    return 0;
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

### 3.4 文件的随机读写

```c++
#include <stdio.h>
int fseek(FILE *stream, long offset, int whence);
功能：移动文件流（文件光标）的读写位置。
参数：
	stream：已经打开的文件指针
	offset：根据whence来移动的位移数（偏移量），可以是正数，也可以负数，如果正数，则相对于whence往右移动，如果是负数，则相对于whence往左移动。如果向前移动的字节数超过了文件开头则出错返回，如果向后移动的字节数超过了文件末尾，再次写入时将增大文件尺寸。
	whence：其取值如下：
		SEEK_SET：从文件开头移动offset个字节
		SEEK_CUR：从当前位置移动offset个字节
		SEEK_END：从文件末尾移动offset个字节
返回值：
	成功：0
	失败：-1
12345678910111213
#include <stdio.h>
long ftell(FILE *stream);
功能：获取文件流（文件光标）的读写位置。
参数：
	stream：已经打开的文件指针
返回值：
	成功：当前文件流（文件光标）的读写位置
	失败：-1
12345678
#include <stdio.h>
void rewind(FILE *stream);
功能：把文件流（文件光标）的读写位置移动到文件开头。
参数：
	stream：已经打开的文件指针
返回值：
	无返回值
1234567
```

##### 示例:

```c
// STATEMENT : 文件的随机读写

#include <stdio.h>

void create_file() {
    FILE *file = fopen("./random.txt", "w");
    if (file == NULL) {
        perror("");
        return;
    }
    const char str[] = "this is a test for a random access file";
    int i = fputs(str, file);
    if (i == 0) {
        printf("write success\n");
    }//write success
    fclose(file);
}

void test_ftell() {
    //查看文件当前的光标位置
    FILE *file = fopen("./random.txt", "r");
    if (file == NULL) {
        perror("");
        return;
    }
    long pos = ftell(file);
    printf("pos=%ld\n", pos);//pos=0
    fclose(file);
}

void test_rewind() {
    //将文件的光标移动到行首
    FILE *file = fopen("./random.txt", "r");
    if (file == NULL) {
        perror("");
        return;
    }
    rewind(file);
    long pos = ftell(file);
    printf("pos=%ld\n", pos);//pos=0
    fclose(file);
}

void test_fseek() {
    //移动光标的位置
    FILE *file = fopen("./random.txt", "r+");
    if (file == NULL) {
        perror("");
        return;
    }
    //先获取当前之战的位置
    long pos = ftell(file);
    printf("pos=%ld\n", pos);//pos=0
    //往当前位置向后移动3个位置
    fseek(file, 3, SEEK_CUR);
    long curr_pos = ftell(file);
    printf("curr_pos = %lu\n", curr_pos); //curr_pos = 3
    //在this后面写入内容
    fseek(file, 5, SEEK_SET);
    char str[] = "hehehe";
    size_t i = fputs(str, file);
    if (i != 0) {
        printf("update success!\n");
    }
    fclose(file);
}

//获取文件的大小
void get_fileSize() {
    FILE *file = fopen("./test.txt", "r");
    if (file == NULL) {
        perror("");
        return;
    }
    fseek(file, 0, SEEK_END);
    long size = ftell(file);
    printf("size of test is %ld byte\n", size);
    //size of test is 842 byte
    fclose(file);
}


int main() {
    create_file();
    //test_ftell();
    // test_rewind();
    test_fseek();
    get_fileSize();
    return 0;
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990
```

### 3.5 获取文件状态

```c++
#include <sys/types.h>
#include <sys/stat.h>
int stat(const char *path, struct stat *buf);
功能：获取文件状态信息
参数：
    path：文件名
    buf：保存文件信息的结构体
返回值：
    成功：0
    失败-1
    
struct stat {
	dev_t         st_dev;         //文件的设备编号
	ino_t         st_ino;          //节点
	mode_t        st_mode;   //文件的类型和存取的权限
	nlink_t       st_nlink;     //连到该文件的硬连接数目，刚建立的文件值为1
	uid_t         st_uid;         //用户ID
	gid_t         st_gid;         //组ID
	dev_t         st_rdev;      //(设备类型)若此文件为设备文件，则为其设备编号
	off_t         st_size;        //文件字节数(文件大小)
	unsigned long st_blksize;   //块大小(文件系统的I/O 缓冲区大小)
	unsigned long st_blocks;    //块数
	time_t        st_atime;     //最后一次访问时间
	time_t        st_mtime;    //最后一次修改时间
	time_t        st_ctime;     //最后一次改变时间(指属性)
};    
1234567891011121314151617181920212223242526
```

##### 示例:

```c
// STATEMENT : 获取文件状态

#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <stdlib.h>

void print_file_info(const char *fileName, struct stat *stat);

void get_info() {
    struct stat *stat1 = malloc(1024);
    const char *fileName = "./test.txt";
    int status = stat("./test.txt", stat1);
    if (status < 0) {
        printf("file not found!\n");
        return;
    }
    //获取文件信息
    print_file_info(fileName, stat1);
    //file_size:842
    //file_crete_time:1606921817
    //file_modify_time:1607005135
    //file_type:3
}

void print_file_info(const char *fileName, struct stat *stat) {
    printf("file_size:%ld\n", stat->st_size);
    printf("file_crete_time:%lld\n", stat->st_ctime);
    printf("file_modify_time:%lld\n", stat->st_atime);
    printf("file_type:%u\n", stat->st_dev);
}

int main() {
    get_info();
    return 0;
}
123456789101112131415161718192021222324252627282930313233343536
```

### 3.6 删除文件、重命名文件名

```c++
#include <stdio.h>
int remove(const char *pathname);
功能：删除文件
参数：
	pathname：文件名
返回值：
	成功：0
	失败：-1
12345678
#include <stdio.h>
int rename(const char *oldpath, const char *newpath);
功能：把oldpath的文件名改为newpath
参数：
    oldpath：旧文件名
    newpath：新文件名
返回值：
    成功：0
    失败： - 1
123456789
```

## 4. 文件缓冲区

### 4.1 缓冲区的概念

缓冲区就是一块临时的空间;

### 4.2 磁盘文件的存取

- 磁盘文件，一般保存在硬盘、U盘等掉电不丢失的磁盘设备中，在**需要时调入内存**
- 在**内存中对文件进行编辑处理后，保存到磁盘中**
- 程序与磁盘之间交互，不是立即完成，**系统或程序可根据需要设置缓冲区**，以提高存取效率;

### 4.3 更新缓冲区

```c++
#include <stdio.h>
int fflush(FILE *stream);
功能：更新缓冲区，让缓冲区的数据立马写到文件中。
参数：
    stream：文件指针
返回值：
    成功：0
    失败：-1
12345678
```

#### 缓冲区刷新的条件

- 缓冲区已满
- 调用fflush函数
- 程序正常结束

