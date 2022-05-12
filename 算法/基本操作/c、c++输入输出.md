## 一、 同一行输入多个数字，数字用空格分开，获取数字；

```c
do

  {

  scanf("%d",&array[i]);

   i++;

  }while(y=getchar()!='\n');     //用于判断是否按了回车
```

 

## 二、 输入一整行数据或者字符；

```c++
while (fgets(line,1024,stdin))

  {
		string[i] = delete_shu(line);
		i++;

  }
```

 

## 三、 输入字符串

```c
 while((ch=getchar())!='\n')

  {

    str1[i++] = ch;

  }

   str1[i] = '\0';
```



## 四、 输入任意长度的一个数组

```c
#include<stdio.h>

#include<stdlib.h>

int main()

{

  int i=0,n=1;

  int *a;

  a=malloc(n*sizeof(int));

  do

  {

    scanf("%d",&a[i++]);

    realloc(a,++n*sizeof(int));

  }while(getchar()!='\n');

  for (i=0;i<n-1;i++)

    printf("%d ",a[i]);

  printf("\n");

}
```

## 五、知道大小输入

```c++


    long long ans, sum;
    int n, minnum;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
```



## scanf用法

在 scanf 中，从键盘输入的一切数据，不管是数字、字母，还是空格、回车、Tab 等字符，都会被当作数据存入缓冲区。存储的顺序是先输入的排前面，后输入的依次往后排。**按回车键的时候 scanf 开始进入缓冲区取数据，从前往后依次取。**

但 scanf 中 %d 只识别“十进制整数”。对 %d 而言，空格、回车、Tab 键都是区分数据与数据的分隔符。当 scanf 进入缓冲区中取数据的时候，如果 %d 遇到空格、回车、Tab 键，那么它并不取用，而是跳过继续往后取后面的数据，直到取到“十进制整数”为止。对于被跳过和取出的数据，系统会将它从缓冲区中释放掉。未被跳过或取出的数据，系统会将它一直放在缓冲区中，直到下一个 scanf 来获取。

但是如果 %d 遇到字母，那么它不会跳过也不会取用，而是直接从缓冲区跳出。所以上面这个程序，虽然 scanf 进入缓冲区了，但用户输入的是字母 a，所以它什么都没取到就出来了，而变量 i 没有值，即未初始化，所以输出就是 –858993460。

但如果将 %d 换成 %c，那么任何数据都会被当作一个字符，不管是数字还是空格、回车、Tab 键它都会取回。

不但如此，前面讲过，你从键盘输入 123，这个不是数字 123，而是字符 '1'、字符 '2' 和字符 '3'，它们依次排列在缓冲区中。因为每个字符变量 char 只能放一个字符。所以输入“123”之后按回车，scanf 开始进入缓冲区，按照次序，先取字符 '1'，如果还要取就再取字符 '2'，以此类推。

## getchar（）

getchar()函数的功能是一个一个地读取你所输入的字符。例如，你从键盘输
入‘aabb’这四个字符，然后按回车，问题来了，getchar()不是一个一个读取吗，你输入一串是什么意思？其实，你按了回车之后，这四个字符会被存储到键盘缓冲区，这个时候你使用getchar()函数，他会从键盘缓冲区里一个一个去读取字符。

**<font color='red'>会读回车</font>**

## fgets（）

在写网络编程时候遇到一个问题：通过fgets读取到了一行输入到缓冲区中，总是要通过strlen()来查下缓冲区中的长度，然后替换。

一开始没懂这个操作，后来查了下资料，原来fgets在读取输入流的时候，会读取你最后的那个回车，也就是'\n'。

比如你现在输入:abcde

实际上保存到缓存区中的是：abcde\n

然而fgets()会自动再补一个‘\0’，也就是说保存到缓冲区send_line中的是abcde\n\0，但是其实我们就没想要这个'\n'这势必会造成数据的不准确，所以我们就要自己来进行一个替换，把最后的'\n'给换成'\0'。

```c++
fgets(char *s, int size, FILE *stream) 
//s 缓冲区
//size 最大读取长度
//文件流 如 stdin
 
 
while (fgets(send_line, MAXLINE, stdin) != NULL) {
        int i = strlen(send_line);
        if (send_line[i - 1] == '\n') {
            send_line[i - 1] = 0;
        }
        ...
}

```

这里还有个疑问，既然都会保存'\n'，那为什么要判断呢？直接把最后一个'\n'进行替换不就好了吗？

我试着输入超过给定size大小的字符串，比如：

![img](https://img-blog.csdnimg.cn/20191209173045639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjY5OTg4,size_16,color_FFFFFF,t_70)

就发现，当你输入超过size的时候，其实不会再保存那个'\n'换行符了，自己就保存成了'\0'。

所以还是记住这种写法！

## gets（）

所以输入一行数据，然后回车结束，系统就默认这一行数据都是输入，**除开末尾的回车字符。**

```c++
#include<stdio.h>
int main(void){
	char a1[10], a2[10],a3[10];
	scanf( "%s%s",a1,a2);
//gets(a2);
	gets(a3);
	puts(a1);
	puts(a2);
	puts(a3);
//	printf("%s\n",ch1);
//	printf("%s",ch2);
	printf("end..."); 
	return 0;
}

```

**a.  scanf是用空格来结束的。 从结果中来看， scanf在读完之后空格还在缓冲区中。 而gets是接收空格的，所以第二张图中输出的 a2 前有空格存在。 
b. 还可以看出scanf也不识别回车， 同样也把回车留在了缓存区中。 而gets是接收回车的， 所以第一张图中的 a3 输出的是回车。
c. gets函数能够接收字符串， 而且不把回车符号留在缓存区中。**

