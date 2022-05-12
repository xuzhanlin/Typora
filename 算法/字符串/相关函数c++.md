[TOC]

### 一、初始化string对象的几种方式

- 1、默认初始化
  string s; //s是一个空串

- 2、使用字符串字面值初始化
  string s1=“hello world”; //拷贝初始化
  string s2(“hello world”); //直接初始化
  注意：s1、s2的内容不包括’\0’

- 3、使用其他字符串初始化
  string s2=s1; //拷贝初始化，s1是string类对象
  string s2(s1); //直接初始化，s1是string类对象

- 4、使用单个字符初始化
  string s(10, ‘a’); //直接初始化，s的内容是aaaaaaaaaa




```c++
#include<iostream>
#include<string>
using namespace std;

int main(){
	string s1;
	string s2="hello world!";
	string s3("hello world!");
	string s4=s2;
	string s5(s2);
	string s6(10,'a');
	
	cout<<"s1: "<<s1<<endl;
	cout<<"s2: "<<s2<<endl;
	cout<<"s3: "<<s3<<endl;
	cout<<"s4: "<<s4<<endl;
	cout<<"s5: "<<s5<<endl;
	cout<<"s6: "<<s6<<endl;
	
	return 0;
}/*output
s1:
s2: hello world!
s3: hello world!
s4: hello world!
s5: hello world!
s6: aaaaaaaaaa

*/

```

### 二. string的大小和容量：：

```c++

1. size()和length()：返回string对象的字符个数，他们执行效果相同。

2. max_size()：返回string对象最多包含的字符数，超出会抛出length_error异常

3. capacity()：重新分配内存之前，string对象能包含的最大字符数

void test2()
{
    string s("1234567");
    cout << "size=" << s.size() << endl;
    cout << "length=" << s.length() << endl;
    cout << "max_size=" << s.max_size() << endl;
    cout << "capacity=" << s.capacity() << endl;

}
```

### 三. string的字符串比较：：

```c++
1. C ++字符串支持常见的比较操作符（>,>=,<,<=,==,!=），甚至支持string与C-string的比较(如 str<”hello”)。  
在使用>,>=,<,<=这些操作符的时候是根据“当前字符特性”将字符按字典顺序进行逐一得 比较。字典排序靠前的字符小，  
比较的顺序是从前向后比较，遇到不相等的字符就按这个位置上的两个字符的比较结果确定两个字符串的大小(前面减后面)
同时，string (“aaaa”) <string(aaaaa)。    

2. 另一个功能强大的比较函数是成员函数 compare()。他支持多参数处理，支持用索引值和长度定位子串来进行比较。 
  他返回一个整数来表示比较结果，返回值意义如下：0：相等 1：大于 -1：小于 (A的ASCII码是65，a的ASCII码是97)
  
  void test3()
{
    // (A的ASCII码是65，a的ASCII码是97)
    // 前面减去后面的ASCII码，>0返回1，<0返回-1，相同返回0
    string A("aBcd");
    string B("Abcd");
    string C("123456");
    string D("123dfg");

    // "aBcd" 和 "Abcd"比较------ a > A
    cout << "A.compare(B)：" << A.compare(B)<< endl;                          // 结果：1

    // "cd" 和 "Abcd"比较------- c > A
    cout << "A.compare(2, 3, B):" <<A.compare(2, 3, B)<< endl;                // 结果：1

    // "cd" 和 "cd"比较 
    cout << "A.compare(2, 3, B, 2, 3):" << A.compare(2, 3, B, 2, 3) << endl;  // 结果：0


    // 由结果看出来：0表示下标，3表示长度
    // "123" 和 "123"比较 
    cout << "C.compare(0, 3, D, 0, 3)" <<C.compare(0, 3, D, 0, 3) << endl;    // 结果：0

}
```

### 四. string的插入：push_back() 和 insert() 和pop_back()

```c++
void  test4()
{
    string s1;

    // 尾插一个字符
    s1.push_back('a');
    s1.push_back('b');
    s1.push_back('c');
    cout<<"s1:"<<s1<<endl; // s1:abc

    // insert(pos,char):在制定的位置pos前插入字符char
    s1.insert(s1.begin(),'1');
    cout<<"s1:"<<s1<<endl; // s1:1abc
}
```

### 五、string拼接字符串：append() & + 操作符

```c++
void test5()
{
    // 方法一：append()
    string s1("abc");
    s1.append("def");
    cout<<"s1:"<<s1<<endl; // s1:abcdef

    // 方法二：+ 操作符
    string s2 = "abc";
    /*s2 += "def";*/
    string s3 = "def";
    s2 += s3.c_str();
    cout<<"s2:"<<s2<<endl; // s2:abcdef
}
```

### 六、 string的遍历：借助迭代器 或者 下标法

```c++
void test6()
{
    string s1("abcdef"); // 调用一次构造函数

    // 方法一： 下标法

    for( int i = 0; i < s1.size() ; i++ )
    {
        cout<<s1[i];
    }
    cout<<endl;

    // 方法二：正向迭代器

    string::iterator iter = s1.begin();
    for( ; iter < s1.end() ; iter++)
    {
        cout<<*iter;
    }
    cout<<endl;

    // 方法三：反向迭代器
    string::reverse_iterator riter = s1.rbegin();
    for( ; riter < s1.rend() ; riter++)
    {
        cout<<*riter;
    }
    cout<<endl;
}
```

### 七、 string的删除：erase()

```c++
1. iterator erase(iterator p);//删除字符串中p所指的字符

2. iterator erase(iterator first, iterator last);//删除字符串中迭代器

区间[first,last)上所有字符

3. string& erase(size_t pos = 0, size_t len = npos);//删除字符串中从索引

位置pos开始的len个字符

4. void clear();//删除字符串中所有字符

void test6()
{
    string s1 = "123456789";


    // s1.erase(s1.begin()+1);              // 结果：13456789
    // s1.erase(s1.begin()+1,s1.end()-2);   // 结果：189
    s1.erase(1,6);                       // 结果：189
    string::iterator iter = s1.begin();
    while( iter != s1.end() )
    {
        cout<<*iter;
        *iter++;
    }
    cout<<endl;

}


```

### 八、 string的字符替换replace：

```c++
1. string& replace(size_t pos, size_t n, const char *s);//将当前字符串

从pos索引开始的n个字符，替换成字符串s

2. string& replace(size_t pos, size_t n, size_t n1, char c); //将当前字符串从pos索引开始的n个字符，替换成n1个字符c

3. string& replace(iterator i1, iterator i2, const char* s);//将当前字符串[i1,i2)区间中的字符串替换为字符串s
void test7()
{
    string s1("hello,world!");

    cout<<s1.size()<<endl;                     // 结果：12
    s1.replace(s1.size()-1,1,1,'.');           // 结果：hello,world.

    // 这里的6表示下标  5表示长度
    s1.replace(6,5,"girl");                    // 结果：hello,girl.
    // s1.begin(),s1.begin()+5 是左闭右开区间
    s1.replace(s1.begin(),s1.begin()+5,"boy"); // 结果：boy,girl.
    cout<<s1<<endl;
}
```

### 十、 string的查找：find

```c++
1. size_t find (constchar* s, size_t pos = 0) const;

  //在当前字符串的pos索引位置开始，查找子串s，返回找到的位置索引，

    -1表示查找不到子串，要抢转成int 

2. size_t find (charc, size_t pos = 0) const;

  //在当前字符串的pos索引位置开始，查找字符c，返回找到的位置索引，

    -1表示查找不到字符

3. size_t rfind (constchar* s, size_t pos = npos) const;

  //在当前字符串的pos索引位置开始，反向查找子串s，返回找到的位置索引，

    -1表示查找不到子串

4. size_t rfind (charc, size_t pos = npos) const;

  //在当前字符串的pos索引位置开始，反向查找字符c，返回找到的位置索引，-1表示查找不到字符

5. size_t find_first_of (const char* s, size_t pos = 0) const;

  //在当前字符串的pos索引位置开始，查找子串s的字符，返回找到的位置索引，-1表示查找不到字符

6. size_t find_first_not_of (const char* s, size_t pos = 0) const;

  //在当前字符串的pos索引位置开始，查找第一个不位于子串s的字符，返回找到的位置索引，-1表示查找不到字符

7. size_t find_last_of(const char* s, size_t pos = npos) const;

  //在当前字符串的pos索引位置开始，查找最后一个位于子串s的字符，返回找到的位置索引，-1表示查找不到字符

8. size_t find_last_not_of (const char* s, size_t pos = npos) const;

 //在当前字符串的pos索引位置开始，查找最后一个不位于子串s的字符，返回找到的位置索引，-1表示查找不到子串
 
 void test8()
{
    string s("dog bird chicken bird cat");

    //字符串查找-----找到后返回首字母在字符串中的下标

    // 1. 查找一个字符串
    cout << s.find("chicken") << endl;        // 结果是：9

    // 2. 从下标为6开始找字符'i'，返回找到的第一个i的下标
    cout << s.find('i',6) << endl;            // 结果是：11

    // 3. 从字符串的末尾开始查找字符串，返回的还是首字母在字符串中的下标
    cout << s.rfind("chicken") << endl;       // 结果是：9

    // 4. 从字符串的末尾开始查找字符
    cout << s.rfind('i') << endl;             // 结果是：18-------因为是从末尾开始查找，所以返回第一次找到的字符

    // 5. 在该字符串中查找第一个属于字符串s的字符
    cout << s.find_first_of("13br98") << endl;  // 结果是：4---b

    // 6. 在该字符串中查找第一个不属于字符串s的字符------先匹配dog，然后bird匹配不到，所以打印4
    cout << s.find_first_not_of("hello dog 2006") << endl; // 结果是：4
    cout << s.find_first_not_of("dog bird 2006") << endl;  // 结果是：9

    // 7. 在该字符串最后中查找第一个属于字符串s的字符
    cout << s.find_last_of("13r98") << endl;               // 结果是：19

    // 8. 在该字符串最后中查找第一个不属于字符串s的字符------先匹配t--a---c，然后空格匹配不到，所以打印21
    cout << s.find_last_not_of("teac") << endl;            // 结果是：21

}
```

### 十一、 string的排序：sort(s.begin(),s.end())

```c++
#include <iostream>
#include <algorithm>
#include <string>
using namespace std;

void test9()
{
    string s = "cdefba";
    sort(s.begin(),s.end());
    cout<<"s:"<<s<<endl;     // 结果：abcdef
}
```

### 十二、 string的分割/截取字符串：strtok() & substr()

```c++
void test10()
{
    char str[] = "I,am,a,student; hello world!";

    const char *split = ",; !";
    char *p2 = strtok(str,split);
    while( p2 != NULL )
    {
        cout<<p2<<endl;
        p2 = strtok(NULL,split);
    }
}

void test11()
{
    string s1("0123456789");
    string s2 = s1.substr(2,5); // 结果：23456-----参数5表示：截取的字符串的长度
    cout<<s2<<endl;
}
```

### 十三、string翻转    reverse

```c++
string s="sjhfjshafjh";
reverse(s.begin(),s.end());
reverse(s.begin()+3,s.end()-2);
```





### 十四、比较字符串 

```c++
允许的比较对象 
1）compare(s2) 其他同类型字符串 
2）compare(p) C 风格字符串 
3）compare(off, cnt, s2) [off, off + cnt) 同 s2 执行比较 
4）compare(off, cnt, s2, off2, cnt2) [off, off + cnt) 同 s2 [off2, cnt2) 执行比较 
5）compare(off, cnt, p) [off, off + cnt) 同 [p , <null>) 执行比较 
6）compare(off, cnt, p, cnt2) [off, off + cnt) 同 [p, p + cnt2) 执行比较 

返回 -1, 0, 1 作为小于、等于和大于的比较结果。 
```

 **六、附加数据 
**1）使用 operator += 接受其他字符串，C 风格字符串和字符 
2）使用 push_back() 在尾部附加字符，并使得通过字符串构造的 back_iterator 可以访问 
3）append() 附加 
    1、append(s) 追加字符串 
    2、append(s, off, cnt) 追加字符串 s [off, off + cnt) 
    3、append(p) 追加字符串 [p, <null>) 
    4、append(p, cnt) 追加字符串 [p, p + cnt) 
    5、append(n, c) 填充 n * c 
    6、append(InF, InL) 追加输入流 [InF, InL) 

4）insert() 插入 
    1、insert(off, s2) 插入字符串 
    2、insert(off, s2, off2, cnt2) 插入字符串 s [off2, off2 + cnt2) 
    3、insert(off, p) 插入字符串 [p, <null>) 
    4、insert(off, p, cnt) 插入字符串 [p, p + cnt)

​    5、insert(off, n, c) 插入 n * c 
​    6、insert(iter) 元素默认值填充 
​    7、insert(iter, c) 插入特定元素 
​    8、insert(iter, n, c) 插入 n*c 
​    9、insert(iter, InF, InL) 插入 [InF, InL) 

5）operator +(a, b) 
字符串关联运算符重载中支持 operator + 的形式 
    1、s + s 
    2、s + p 
    3、s + c 
    4、p + s 
    5、c + s 

**七、查找、替换和清除 
**1）find() 查找 
    1、find(c, off) 在 s [off, npos) 中查找 c 
    2、find(p, off, n) 在 s [off, npos) 中查找 [p, p + n) 
    3、find(p, off) 在 s [off, npos) 中查找 [p, <null>) 
    4、find(s2, off) 在 s [off, npos) 中查找 s2 

2）find() 的变种 
    1、rfind() 具有 find() 的输入形式，反序查找 
    2、find_first_of() 具有 find() 的输入形式，返回第一个匹配的索引 
    3、find_last_of() 具有 find() 的输入形式，返回倒数第一个匹配的索引 
    4、find_first_not_of() 具有 find() 的输入形式，返回第一个不匹配的索引 
    5、find_last_not_of() 具有 find() 的输入形式，返回倒数第一个不匹配的索引 

3）replace() 替换 
    1、replace(off, cnt, s2) 将 s [off, off + cnt) 替换成 s2 
    2、replace(off, cnt, s2, off2, cnt2) 将 s [off, off + cnt) 替换成 s2 [off2, off2 + cnt2)
    3、replace(off, cnt, p) 将 s [off, off + cnt) 替换成 [p, <null>) 
    4、replace(off, cnt, p, cnt2) 将 s [off, off + cnt) 替换成 [p, p + cnt2)

​    5、replace(off, cnt, n, c) 将 s [off, off + cnt) 替换成 c * n 

使用迭代器的情况： 
    6、replace(InF, InL, s2) 将 [InF, InL) 替换成 s2 
    7、replace(InF, InL, p) 将 [InF, InL) 替换成 [p, <null>) 
    8、replace(InF, InL, p, cnt) 将 [InF, InL) 替换成 [p, p + cnt) 
    9、replace(InF, InL, n, c) 将 [InF, InL) 替换成 n * c 
    10、replace(InF, InL, InF2, InL2) 将 [InF, InL) 替换成 [InF2, InL2) 

4）erase() 删除 
    1、erase(off, cnt) 从字符串 s 中删除 s [off, off + cnt) 
    2、erase(iter) 从字符串 s 中删除 *iter 
    3、erase(ItF, ItL) 从字符串 s 中删除 [ItF, ItL) 

**八、取出字符串 
**1）取得 C 风格字符串 
c_str() 返回常量类型的 C 风格字符串指针，copy(ptr, cnt, off = 0) 则将指定大小的字符串复制到特定指针。data() 在 Visual C++ 7.1 中仅仅调用了 c_str() 实现。 
2）取得子字符串 
substr(off, cnt) 取得 s [off, off + cnt) 的副本。 
3）复制子字符串 
copy(p, off, cnt) 将 s [off, off + cnt) 复制到 p。 


**九、字符串的缓冲区管理 
**字符串具有类似 std::vector 的缓冲区管理界面。 
size() 取得有效元素长度 
max_size() 取得当前内存分配器能分配的有效空间 
reserve() 为缓冲区预留空间 
capacity() 取得缓冲区的容量 
resize() 重设串的长度，可以为其指定初始化值 

 **十、定义输入迭代器的尾端 
**向 istream_iterator 传递输入流对象以创建输入迭代器，输入迭代器持有输入流对象的指针，默认创建和读取流失败的情况下该指针被设置为 0。并且在实现输入迭代器间的 operator == 相等运算时，进行持有的流对象指针的相等比较，这样，默认创建的输入迭代器将被用于匹配输入流的结束。 

\* 当输入流读取失败，用户执行 if, while 条件判断时，实际上先将判断值转换成 void* 类型，或者根据 operator ! 运算符的返回结果，对输入流重载 operator void* 和 operator ! 运算符，可以定义输入流在布尔表达式中的行为，使得当流读取失败的情况下，输入迭代器可以通过布尔表达式来确认，而不是显式访问 fail() 成员函数. 