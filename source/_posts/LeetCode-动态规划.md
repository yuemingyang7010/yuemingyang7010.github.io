---
title: '动态规划'
date: 2019-06-29 13:52:09
tags:
	- 动态规划刷题
categories: 
	- 数据结构与算法
	- 算法刷题
---



### 最长公共子序列问题(LCS问题)

给定两个字符串A和B，长度分别为m和n，要求找出它们最长的公共子序列，并**返回子序列**。例如：

　　A = "Hel**lo**W**o**rld"

　   B = "**loo**p"

当我们要求**dp [i] [j]**，我们要先判断**A的第i个元素B的第j个元素**是否相同即判断**A[i - 1]**和 **B[j -1]**是否相同，如果相同它就是**dp [i-1] [j-1]+ 1**，相当于在两个字符串都去掉一个字符时的**最长公共子序列**再加 **1**；否则**最长公共子序列**取**dp[i] [j - 1]** 和**dp[i - 1] [j]**中大者。所以整个问题的初始状态为：															

```
dp[i] [0]=0,dp[0] [j]=0
```

相应的状态转移方程为：

{%asset_img 01.png%}

```c++
#include <iostream>
#include <vector>
#include <stdlib.h>
using namespace std;
void LCS(const char* str1,const char* str2,string& str)
{
	int m = (int)strlen(str1);
	int n = (int)strlen(str2);
	vector<vector<char> > dp(m+1, vector<char>(n+1)); 
//第一步：填二维表格
	int i, j;
	for (i = 0; i <= m; i++)//初始状态
		dp[i][0] = 0;
	for (i = 0; i <= n; i++)
		dp[0][i] = 0;
	for (i = 1; i <= m; i++)
		for (j = 1; j <= n; j++)
		{
			if (str1[i - 1] == str2[j - 1])//判断A的第i个字符和B的第j个字符是否相同
				dp[i][j] = dp[i - 1][j - 1] + 1;
			else
				dp[i][j] = dp[i - 1][j] > dp[i][j - 1]? dp[i - 1][j]: dp[i][j - 1];
		}
//第二步：从左下角找到朝左上部分遍历，不一定最终到达左上角，只要i或者j等于0则终止
	i = m;
	j = n;    
	while ((i != 0)&&(j != 0))
	{
		if (str1[i-1] == str2[j-1])
		{
			str.push_back(str1[i]);
			i--;
			j--;
		} 
		else
		{
			if (dp[i][j - 1] > dp[i - 1][j])
				j--;
			else
				i--;
		}
	}
	reverse(str.begin(),str.end());
}

int main() {
	const char* str1="HelloWorld";
	const char* str2 = "loop";
	string str;
	LCS(str1,str2,str);
	cout << str.c_str() << endl;
	return 0;
}
```

遍历的过程：

str1[i-1] == str2[j-1] 成立时，向左上角遍历

不成立时，dp[i] [j - 1] > dp[i - 1] [j] 成立则朝左遍历，否则朝上遍历

{%asset_img 03.png%}



### 最长公共子串问题

给定两个字符串A和B，长度分别为m和n，要求找出它们最长的公共子串，并返回其长度。例如：

A = "Hel**lo**World"

B = "**lo**op"

子序列和子串的区别：**子序列和子串都是字符集合的子集，但是子序列不一定连续，但是子串一定是连续的**。同样地，这里只给出动态规划的解法：定义**dp[i] [j]**表示以A中第i个字符结尾的子串和B中第j个字符结尾的子串的的最大公共子串(**其中A中第i个字符和B中第J个字符指**)的长度。

当我们要求**dp[i] [j]**，我们要先判断**A的第i个元素B的第j个元素**是否相同即判断**A[i - 1]**和 **B[j -1]**是否相同，如果相同它就是**dp[i - 1] [j- 1] + 1**，相当于在两个字符串都去掉一个字符时的**最长公共子串**再加 **1**；否则**最长公共子串**取0。所以整个问题的初始状态为：

```
dp[i] [0]=0,dp[0] [j]=0
```

相应的状态转移方程为：

{%asset_img 02.png%}

代码的实现如下：

```c++
#include <iostream>
#include <vector>
#include <stdlib.h>
using namespace std;
string findLongest(const char* str1,const char* str2)
{
	int m = (int)strlen(str1);
	int n = (int)strlen(str2);
	int result = 0;
	int imax = 0;
	int jmax = 0;
	string str;
	vector<vector<char> > dp(m+1, vector<char>(n+1)); 
	int i, j;
	for (i = 0; i <= m; i++)//初始状态
		dp[i][0] = 0;
	for (i = 0; i <= n; i++)
		dp[0][i] = 0;
	for (i = 1; i <= m; i++)
		for (j = 1; j <= n; j++)
		{
			if (str1[i - 1] == str2[j - 1]) {//判断A的第i个字符和B的第j个字符是否相同
				dp[i][j] = dp[i - 1][j - 1] + 1;
				if (dp[i][j] > result)//当前dp[i][j]大于result，则更新公共子串最大长度，及其对应坐标
				{
					result = dp[i][j];
					imax = i;
					jmax = j;
				}
			}
			else
				dp[i][j] = 0;
		}
	while (result != 0)
	{
		str.push_back(str1[imax-1]);
		imax--;
		jmax--;
		result--;
	}
	reverse(str.begin(), str.end());
	return str;
}

int main() {
	const char* str1="HelloWorld";
	const char* str2 = "loop";
	string resultStr = findLongest(str1,str2);
	cout << resultStr.c_str() << endl;
	return 0;
}
```

遍历的过程：

从dp最大的位置沿着左上角遍历result个字符

{%asset_img 04.png%}

