**广度优先搜索**会根据离起点的距离，按照从近到远的顺序对各节点进行搜索。而深度优先搜索会沿着一条路径不断往下搜索直到不能再继续为止，然后再折返，开始搜索下一条路径。



在广度优先搜索中，有一个保存候补节点的**队列**，队列的性质就是**先进先出**，即先进入该队列的候补节点就先进行搜索。

### 模板

```c++
#include<cstdio>
#include<cstring>
#include<queue>
#include<algorithm>
using namespace std;
const int maxn=100;
bool vst[maxn][maxn]; // 访问标记
int dir[4][2]={0,1,0,-1,1,0,-1,0}; // 方向向量
 
struct State // BFS 队列中的状态数据结构
{
	int x,y; // 坐标位置
	int Step_Counter; // 搜索步数统计器
};
 
State a[maxn];
 
bool CheckState(State s) // 约束条件检验
{
	if(!vst[s.x][s.y] && ...) // 满足条件		
		return 1;
	else // 约束条件冲突
	return 0;
}
 
void bfs(State st)
{
	queue <State> q; // BFS 队列
	State now,next; // 定义2 个状态，当前和下一个
	st.Step_Counter=0; // 计数器清零
	q.push(st); // 入队	
	vst[st.x][st.y]=1; // 访问标记
	while(!q.empty())
	{
		now=q.front(); // 取队首元素进行扩展
		if(now==G) // 出现目标态，此时为Step_Counter 的最小值，可以退出即可
		{
			...... // 做相关处理
			return;
		}
	for(int i=0;i<4;i++)
	{
		next.x=now.x+dir[i][0]; // 按照规则生成	下一个状态
		next.y=now.y+dir[i][1];
		next.Step_Counter=now.Step_Counter+1; // 计数器加1
		if(CheckState(next)) // 如果状态满足约束条件则入队
		{
			q.push(next);
			vst[next.x][next.y]=1; //访问标记
		}
	}
	q.pop(); // 队首元素出队
	}
 return;
}
 
int main()
{
......
 return 0;
}

```

```c++
class Solution {
public:
    int col;
    int row;
    static constexpr int seg[4][2]={{1,0},{-1,0},{0,1},{0,-1}};
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
            row=mat.size();
            col=mat[0].size();
            vector<vector<int>> dist(row,vector<int>(col));
            vector<vector<int>> seen(row,vector<int>(col));
            queue<pair<int,int>> q;
            
            for(int i=0;i<row;i++)
            {
                for(int j=0;j<col;j++)
                {
                    if(mat[i][j]==0)
                    {
                       q.emplace(i,j);
                       seen[i][j]=1;
                    }
                    
                }
            }
            while(!q.empty())
            {
                auto[a,b]=q.front();
                q.pop();
                for(int k=0;k<4;k++)
                {
                    int aa=a+seg[k][0];
                    int bb=b+seg[k][1];
                    if(aa>=0&&bb>=0&&aa<row&&bb<col&&seen[aa][bb]==0)
                    {
                        seen[aa][bb]=1;
                        dist[aa][bb]=dist[a][b]+1;
                        q.emplace(aa,bb);
                    }
                }

            }
            return dist;
    }
};
```

