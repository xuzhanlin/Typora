### 基本概念

**深度优先搜索算法**（Depth First Search，简称DFS）：一种用于遍历或搜索树或图的算法。 沿着树的深度遍历树的节点，尽可能深的搜索树的分支。当节点v的所在边都己被探寻过或者在搜寻时结点不满足条件，搜索将回溯到发现节点v的那条边的起始节点。整个进程反复进行直到所有节点都被访问为止。属于盲目搜索,最糟糕的情况算法时间复杂度为O(!n)。

### 算法思想

回溯法（探索与回溯法）是一种选优搜索法，又称为试探法，按选优条件向前搜索，以达到目标。但当探索到某一步时，发现原先选择并不优或达不到目标，就退回一步重新选择，这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个状态的点称为“回溯点”。 

### 基本模板

1. ```C++
   int check(参数)
   {
       if(满足条件)
           return 1;
       return 0;
   }
    
   void dfs(int step)
   {
           判断边界
           {
               相应操作
           }
           尝试每一种可能
           {
                  满足check条件
                  标记
                  继续下一步dfs(step+1)
                  恢复初始状态（回溯的时候要用到）
           }
   }   
   
   ```

   2.

   ```c++
   #include<iostream>
   #include<cstdio>
   #include<cstring>
   #include<cstdlib>
   using namespace std;
   const int maxn=100;
   bool vis[maxn][maxn]; // 访问标记
   int map[maxn][maxn]; // 坐标范围
   int dir[4][2]={{0, -1}, {1, 0}, {0, 1}, {-1, 0}}; // 方向矩阵
   //这部分也可以只写边界条件的判断
   bool CheckState(int x,int y) // 边界条件和约束条件的判断
   {
   	if(!vis[x][y] && ...) // 满足条件
   		return 1;
   	else // 与约束条件冲突
   		return 0;
   }
   void dfs(int x,int y)
   {
   	vis[x][y]=1; // 标记该节点被访问过
   	if(map[x][y]==T) // 出现目标态T
   	{
   		...... // 做相应处理
   		return;
   	}
   	for(int i=0;i<4;i++)
   	{
   		int tx = x + dir[i][0];
   		int ty = y + dir[i][1];
   		if(CheckState(tx, ty)) // 按照规则生成下一个节点
   			dfs(tx, ty);
   	}
   	return; // 没有下层搜索节点，回溯
   }
   int main()
   {
   	......
   	return 0;
   }
   
   ```

   

   ### 问题示例

   1、全排列问题

   从n个不同元素中任取m（m≤n）个元素，按照一定的顺序排列起来，叫做从n个不同元素中取出m个元素的一个排列。当m=n时所有的排列情况叫全排列。

   ```c++
   //全排列问题
   #include<stdio.h>
   #include<string.h>
    
   int n;
   char  a[15];
   char re[15];
   int vis[15];
   //假设有n个字符要排列，把他们依次放到n个箱子中
   //先要检查箱子是否为空，手中还有什么字符，把他们放进并标记。
   //放完一次要恢复初始状态，当到n+1个箱子时，一次排列已经结束
   void dfs(int step)
   {
       int i;
       if(step==n+1)//判断边界
       {
           for(i=1;i<=n;i++)
               printf("%c",re[i]);
           printf("\n");
           return ;
       }
       for(i=1;i<=n;i++)//遍历每一种情况
       {
           if(vis[i]==0)//check满足
           {
               re[step]=a[i];
               vis[i]=1;//标记
               dfs(step+1);//继续搜索
               vis[i]=0;//恢复初始状态
           }
       }
       return ;
   }
    
   int main(void)
   {
       int T;
       scanf("%d",&T);
       getchar();
       while(T--)
       {
           memset(a,0,sizeof(a));
           memset(vis,0,sizeof(vis));//对存数据的数组分别初始化
           scanf("%s",a+1);
           n=strlen(a+1);
           dfs(1);//从第一个箱子开始
       }
       return 0;
   }
   
   ```

   2、一个环由个圈组成，把自然数1，2，…，N分别放在每一个圆内，数字的在两个相邻圈之和应该是一个素数。 注意：第一圈数应始终为1。

   input: N(0~20)

   output:输出格式如下所示的样品。每一行表示在环中的一系列圆号码从1开始顺时针和按逆时针方向。编号的顺序必须满足上述要求。打印解决方案的字典顺序。

   ```c++
   //Prime Ring Problem
   //与上面的全排列问题其实思路差不多，只是需要判断的条件比较多
   //化大为小
    
   #include<stdio.h>
   #include<string.h>
   #include<stdlib.h>
    
   int book[25];
   int result[25];
   int n;
   int num;
   //判断是否为素数
   int prime(int n)
   {
       if(n<=1)
           return 0;
       int i;
       for(i=2;i*i<=n;i++)
       {
           if(n%i==0)
               break;
       }
       if(i*i>n)
           return 1;
       return 0;
   }
   //判断是否能将当前的数字放到当前的圈内
   int check(int i,int step)
   {
       if((book[i]==0) && prime(i+result[step-1])==1)
       {
           if(step==n-1)
           {
               if(!prime(i+result[0]))
                   return 0;
           }
           return 1;
       }
       return 0;
   }
    
   void dfs(int step)
   {
       if(step==n)//判断边界
       {
           int a;
           printf("%d",result[0]);
           for(a=1;a<n;a++)
           {
               printf(" %d",result[a]);
           }
           printf("\n");
           return ;
       }
       int i;
       for(i=2;i<=n;i++)//遍历每一种情况
       {
           if(check(i,step))//check是否满足
           {
               book[i]=1;//标记
               result[step]=i;//记录结果
               dfs(step+1);//继续往下搜索
               book[i]=0;//恢复初始状态
           }
       }
   }
    
   int main(void)
   {
    
       while(scanf("%d",&n)!=EOF)
       {
           num++;
           memset(result,0,sizeof(result));
           memset(book,0,sizeof(book));
           result[0]=1;
           printf("Case %d:\n",num);//格式比较容易出错
           dfs(1);
           printf("\n");
       }
       return 0;
   }
   
   ```

   3、油田问题

   问题：GeoSurvComp地质调查公司负责探测地下石油储藏。 GeoSurvComp现在在一块矩形区域探测石油，并把这个大区域分成了很多小块。他们通过专业设备，来分析每个小块中是否蕴藏石油。如果这些蕴藏石油的小方格相邻，那么他们被认为是同一油藏的一部分。在这块矩形区域，可能有很多油藏。你的任务是确定有多少不同的油藏。

   input: 输入可能有多个矩形区域（即可能有多组测试）。每个矩形区域的起始行包含m和n，表示行和列的数量，

   1<=n,m<=100，如果m =0表示输入的结束，接下来是n行，每行m个字符。每个字符对应一个小方格，并且要么是’*’，代表没有油，要么是’@’，表示有油。

   output: 对于每一个矩形区域，输出油藏的数量。两个小方格是相邻的，当且仅当他们水平或者垂直或者对角线相邻（即8个方向）。

   ```c++
   //A - Oil Deposits 
   #include<stdio.h>
   #include<string.h>
   #include<stdlib.h>
    
   char a[105][105];
   int n,m,result;
   int dir[8][2]={{1,0},{-1,0},{0,1},{0,-1},{1,1},{-1,-1},{1,-1},{-1,1}};//表示8个方向
    
   int check(int x,int y)//检查是否有油田
   {
       if(x>=0&&x<m&&y>=0&&y<n&&a[x][y]=='@')
           return 1;
       return 0;
   }
    
   int dfs(int x, int y)
   {
       int i,xx,yy;
       if(check(x,y))
       {
           a[x][y]='.'; //统计之后就可以把该油田标记，且不用恢复（要不会重复），
                       //也可以用一个数组来存每个点的访问情况，但是感觉没必要，浪费空间
           for(i=0;i<8;i++)
           {
               xx=x+dir[i][0];
               yy=y+dir[i][1];
               dfs(xx,yy);//依次检查8个方向
           }
           return 1;
       }
       return 0;
   }
    
   int main(void)
   {
       int i,j;
       while(scanf("%d %d",&m,&n)==2)
       {
           if(m==0&&n==0)
               break;
           result = 0;
           memset(a,0,sizeof(a));
           for(i=0;i<m;i++)
               scanf("%s",a[i]);
           for(i=0;i<m;i++)//在每一个点都搜索一次
           {
               for(j=0;j<n;j++)
               {
                   if(dfs(i,j))//找到油田就可以将结果加1
                       result++;
               }
           }
           printf("%d\n",result);
       }
       return 0;
   }
   
   ```

   4、棋盘问题

   问题：在一个给定形状的棋盘（形状可能是不规则的）上面摆放棋子，棋子没有区别。要求摆放时任意的两个棋子不能放在棋盘中的同一行或者同一列，请编程求解对于给定形状和大小的棋盘，摆放k个棋子的所有可行的摆放方案C。

   input： 输入含有多组测试数据。 每组数据的第一行是两个正整数，n k，用一个空格隔开，表示了将在一个n*n的矩阵内描述棋盘，以及摆放棋子的数目。 n <= 8 , k <= n 当为-1 -1时表示输入结束。 随后的n行描述了棋盘的形状：每行有n个字符，其中 # 表示棋盘区域， . 表示空白区域（数据保证不出现多余的空白行或者空白列）。

   output：对于每一组数据，给出一行输出，输出摆放的方案数目C （数据保证C<2^31）。

   ```c++
   #include<stdio.h>
   #include<string.h>
   #include<stdlib.h>
    
   int n, k, ans;
   char str[10][10];
   int vis[100];
    
   void dfs(int r, int k)
   {
       if(k==0)//判断边界，此时棋子已经放完
       {
           ans++;
           return;
       }
    
       for(int i=r; i<n; i++)//每次都从放过棋子下一行开始搜索，保证不重复
       {
           for(int j=0; j<n; j++)
           {
               //循环保证行不重复，check保证列不重复
               if(str[i][j]=='.' || vis[j]==1)
                   continue;//不满足条件直接跳过
               vis[j] = 1;//标记
               dfs(i+1, k-1);//继续下一次标记
               vis[j] = 0;//恢复初始状态
           }
       }
   }
    
   int main(void)
   {
       while(1)
       {
           scanf("%d %d", &n, &k);
           getchar();
           if(n==-1 && k==-1) 
               break;
           memset(str, '\0', sizeof(str));
           memset(vis, 0, sizeof(vis));
           ans = 0;
    
           for(int i=0; i<n; i++)
           {
               for(int j=0; j<n; j++)
                   str[i][j] = getchar();
               getchar();
           }
    
           dfs(0, k);//从第0行开始放，此时手中还剩k个棋子
           printf("%d\n", ans);
       }
       return 0;
   }
   
   ```

