### vector实现三维数组   同一元素

```c++
#include <iostream>
#include <vector>
int main()
{
    std::vector<std::vector<std::vector<int> > > a(2);//创建2个vector<vector<int> >类型的数组
    for (int n = 0; n < 2; n++)
    {
        a[n].resize(4);
    }//要先对二维数组设定大小

    for (int i = 0; i < 2; i++)
    {
        for (int j = 0; j < 2; j++)
        {
            a[i][j].resize(2);
        }//之后才能对三维数组设定大小，否则内存出错
    }

    int m = 1;
    for (int i = 0; i < 2; i++)
    {
        for (int j = 0; j < 2; j++)
        {
            for (int k = 0; k < 2; k++)
            {
                a[i][j][k] = m++;
            }
        }
    }

    for (int i = 0; i < 2; i++)
    {
        for (int j = 0; j < 2; j++)
        {
            for (int k = 0; k < 2; k++)
            {
                std::cout << a[i][j][k] << std::endl;
            }
        }
    }
    std::cin.get();
    return 0;
}
```

###  三维vector    不同元素

```c++
struct StationInformation {
        int id;
        int time;
    };
    struct station {
        string name;
        vector<struct StationInformation> p;
    };
    vector<struct station> All;
 

    void checkIn(int id, string stationName, int t) {
        struct StationInformation informations;
        informations.id = id;
        informations.time = t;
        struct station c;
        c.name = stationName;
        c.p.push_back(informations);
        All.push_back(c);
    }

```

### vector 基本用法

#### 一、vector 的初始化：可以有五种方式,举例说明如下

```c++
    (1) vector<int> a(10); //定义了10个整型元素的向量（尖括号中为元素类型名，它可以是任何合法的数据类型），但没有给出初值，其值是不确定的。
   （2）vector<int> a(10,1); //定义了10个整型元素的向量,且给出每个元素的初值为1
   （3）vector<int> a(b); //用b向量来创建a向量，整体复制性赋值
   （4）vector<int> a(b.begin(),b.begin+3); //定义了a值为b中第0个到第2个（共3个）元素
   （5）int b[7]={1,2,3,4,5,9,8};
        vector<int> a(b,b+7); //从数组中获得初值
```

#### 二、vector对象的几个重要操作，举例说明如下：

```c++
    （1）a.assign(b.begin(), b.begin()+3); //b为向量，将b的0~2个元素构成的向量赋给a
    （2）a.assign(4,2); //是a只含4个元素，且每个元素为2
    （3）a.back(); //返回a的最后一个元素
    （4）a.front(); //返回a的第一个元素
    （5）a[i]; //返回a的第i个元素，当且仅当a[i]存在2013-12-07
    （6）a.clear(); //清空a中的元素
    （7）a.empty(); //判断a是否为空，空则返回ture,不空则返回false
    （8）a.pop_back(); //删除a向量的最后一个元素
    （9）a.erase(a.begin()+1,a.begin()+3); //删除a中第1个（从第0个算起）到第2个元素，也就是说删除的元素从a.begin()+1算起（包括它）一直到a.begin()+3（不包括它）
    （10）a.push_back(5); //在a的最后一个向量后插入一个元素，其值为5
    （11）a.insert(a.begin()+1,5); //在a的第1个元素（从第0个算起）的位置插入数值5，如a为1,2,3,4，插入元素后为1,5,2,3,4
    （12）a.insert(a.begin()+1,3,5); //在a的第1个元素（从第0个算起）的位置插入3个数，其值都为5
    （13）a.insert(a.begin()+1,b+3,b+6); //b为数组，在a的第1个元素（从第0个算起）的位置插入b的第3个元素到第5个元素（不包括b+6），如b为1,2,3,4,5,9,8         ，插入元素后为1,4,5,9,2,3,4,5,9,8
    （14）a.size(); //返回a中元素的个数；
    （15）a.capacity(); //返回a在内存中总共可以容纳的元素个数
    （16）a.resize(10); //将a的现有元素个数调至10个，多则删，少则补，其值随机
    （17）a.resize(10,2); //将a的现有元素个数调至10个，多则删，少则补，其值为2
    （18）a.reserve(100); //将a的容量（capacity）扩充至100，也就是说现在测试a.capacity();的时候返回值是100.这种操作只有在需要给a添加大量数据的时候才         显得有意义，因为这将避免内存多次容量扩充操作（当a的容量不足时电脑会自动扩容，当然这必然降低性能） 
    （19）a.swap(b); //b为向量，将a中的元素和b中的元素进行整体性交换
    （20）a==b; //b为向量，向量的比较操作还有!=,>=,<=,>,<
```

#### 三、几种重要的算法，使用时需要包含头文件： 

**#include <algorithm>**

```c++
#include<algorithm>
    
（1）sort(a.begin(),a.end()); //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素进行从小到大排列
（2）reverse(a.begin(),a.end()); //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素倒置，但不排列，如a中元素为1,3,2,4,倒置后为4,2,3,1
（3）copy(a.begin(),a.end(),b.begin()+1); //把a中的从a.begin()（包括它）到a.end()（不包括它）的元素复制到b中，从b.begin()+1的位置（包括它）开        始复制，覆盖掉原有元素
（4）find(a.begin(),a.end(),10); //在a中的从a.begin()（包括它）到a.end()（不包括它）的元素中查找10，若存在返回其在向量中的位置
    vector<int>::iterator it = find(vec.begin(), vec.end(), 6);

（5）STL中的 upper_bound(), lower_bound() 库函数
```

### 四、reserve与resize的区别

看到方法2，我们可能会有疑问，它不是相当于

```c++
vector<int> vec(1000),，然后调用1000次 vec.push_back(***)；

或者

int arr[1000]，然后调用arr[i]=***；
```

么？No！效果是一样的，都是一次性分配内存，效率很高，但是它们同方法2存在本质的区别。**reserve方法是只分配内存，不创建对象**，而这两种方法都创建了1000个int类型的对象。而**resize函数正是在分配内存的同时会创建对象**。

感觉它们的功能都大同小异，那么什么情况下使用哪一个函数比较好呢？我自己的意见是在能用reserve的情况下都用reserve，只能用resize的情况下用resize，因为使用reserve只需要赋一次值（调用push_back的时候），而resize需要赋两次值（初始化和调用push_back的时候）。哪种情况下只能用resize呢？就是当vector已经有数据，需要扩展数据的时候。

### 五、其它

```c++
vector<pair<int,int>> arr;
int x=1.y=3;
arr.emplace_back(x,y);

bool cmp(const pair<int, int> &a,const pair<int, int> &b){
    return a.first>b.first;
}
sort(arr.begin(),arr.end(),cmp);

arr[0].second
    
vector<pair<int,int>> ans;   
ans.push_back(arr[0]);
```

