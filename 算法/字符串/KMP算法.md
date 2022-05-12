KMP算法是一种改进的字符串匹配算法，由D.E.Knuth，J.H.Morris和V.R.Pratt提出的，因此人们称它为克努特—莫里斯—普拉特操作（简称KMP算法）。KMP算法的核心是利用匹配失败后的信息，尽量减少模式串与主串的匹配次数以达到快速匹配的目的。具体实现就是通过一个next()函数实现，函数本身包含了模式串的局部匹配信息。KMP算法的时间复杂度O(m+n)。

##### 求next数组的代码：

```c++
//求解next数组
void Cal_Next(char* T,int *next)
{
	int k = -1;//k控制匹配过程中的下标变换
	next[0] = k;//next[0]必定为-1
	//i控制求解过程中的下标变化，每一次大循环，一定会得到一个next值
	//i此时等于多少，就是在求next数组中该下标所对应的值
	for (int i = 1; i < strlen(T); i++)
	{
		//如果k=0，那么说明k要从第一个字符开始匹配，直接跳到下面判断第一个字符是否等于当前要匹配的字符
		//此时的k是上一次匹配完后k的位置，如果k处的值不等于此时i处的值，那么k要不断的回溯，直到找到合适的能接上的值
		while (k > -1 && T[k+1] != T[i])
		{
			k = next[k];
		}
		if (T[k+1] == T[i])//判断以下是否可以接上，可以就k+1
		{
			k++;
		}
		next[i] = k;//得到next值
	}
}

```



##### 返回第一个找到的下标：

```c++
int Index_KMP(char* S, char* T, int pos)
{
	int next[MAXSIZE];
	Cal_Next(T, next);
	int i = pos;//i控制主串下标
	int j = 0;//j控制子串下标
	while (i < strlen(S) && j < strlen(T))
	{
		if (j==0||S[i] == T[j])//如果子串回溯到0了，同样往后比较一位
		{
			i++;
			j++;
		}
		else
		{
			j = next[j-1]+1;//注意是前一位的next值加1
		}
	}
	if (j >= strlen(T))
	{
		return i-strlen(T);
	}
	else
	{
		return -1;
	}
}

```

