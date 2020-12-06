---
title: '字符串'
date: 2019-06-29 13:52:09
tags:
	- 字符串刷题
categories: 
	- 数据结构与算法
	- 算法刷题
---

[C语言字符串函数参考1](http://baijiahao.baidu.com/s?id=1605574559806098900&wfr=spider&for=pc)

[C语言字符串函数参考2](https://www.cnblogs.com/xionghj/p/4443891.html)

[C++语言字符串函数参考](http://c.biancheng.net/cpp/biancheng/view/3284.html)

# 字符串刷题方法

字符串循环移位
LCS最长递增子序到   **去动态规划看**
字符串全排到
Manacher算法
KMP模式串匹配
	附：BM算法
三字母字符串组合



## 字符串循环左移

题目：给定一个字符串S[0..N-1]，要求把S的前k个字符移动到S的尾部，如把字符串“abcdef”前面的2个字符‘a’、b’移动到字符串的尾部，得到新字符串“cdefab”：即字符串循环左移k。
多说一句：循环左移n+k位和k位的效果相同，循环左移k位等价于循环右移n-k位。
算法要求：
**时间复杂度为O（n），空间复杂度为O（1）。**

分析：

**暴力移位法**
每次循环左移1位，调用k次即可
时间复杂度O（kN），空间复杂度O（1）

例如：

```c++
int temp = S[0];
for(int j=0;j<k;j++){
	for(int i=1;i<len;i++){
		S[i-1] = S[i];
	}
}
```

**三次拷贝**
S[0...k]→T[0...k]
S[k+1..N-1]→S[0..N-k-1]
T[0...k]→S[N-k...N-1]
时间复杂度O（N），空间复杂度O（k）

**优雅一点的算法**
(1)（X‘Y’)‘=YX

​		如：abcdef 

​		X=ab         X'=ba 

​		Y=cdef      Y'=fedc

​	 （X‘Y’)‘=（bafedc）’ = cdefab

(2)时间复杂度O（N），空间复杂度O（1）

​		该问题会在“完美洗牌”算法中再次遇到。

```c++
//字符串翻转
void ReverseString(char* s,int left,int right){
 	while(left < right){
        char tem = s[left];
        s[left++] = s[right];
        s[right--] = t;
    }   
}
//n为字符串长度，m为左移多少位
void LeftRotateString(char* s,int n,int m){
    m %= n;
    ReverseString(s,0,m-1);
    ReverseString(s,m,n-1);
    ReverseString(s,0,n-1);
}
```

**该方法特别适合，反转 “I love china”为“china love I”**



## 最长回文子串(leetcode 5)

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：

输入: "cbbd"
输出: "bb"

思路：[参考](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247484049&idx=1&sn=b3ee1a909d0c75cb9ef57df69ca36f5c&chksm=972ad3eba05d5afd63dcb87c78a3b99cd6312e86c26d3727a7c7e01f0ed9a5f4f81edbee06f9&mpshare=1&scene=1&srcid=0719xSkvzvmyhSB5d0cGY6Au&key=034516426b2066d0e0aa6ab7340f5165b69a49c8183a1b4a232fae513c14ca25e69fa8220126e8ae5c517f68513d650b40ea657e95aed7b3002b7281a4c0fec8b6e601f0a455962f47f12cab9c546f51&ascene=1&uin=MjUzODM5ODQwNA%3D%3D&devicetype=Windows+10&version=62060833&lang=zh_CN&pass_ticket=x3FegFPx%2Bvw0qa34JSQ8rVDFtlaR46UnZVCzg53idyupd1SCO5oQv401BqivS7%2Fn)

**方法一：Manacher 算法求最长回文子串:  时间复杂度O(N)**

```c++
class Solution {
public:
	string longestPalindrome(string s) {
		if (s == "")
			return "";
		string str = "@";
		string result;
		int len = s.length();
		for (int i = 0; i < len; i++)
			str = str + "#" + s[i];
		str += "#$";
		vector<int> p(str.length(), 1);
		manacher(str, p);
		int max = 1;
		int imax;
		for (int i = 0; i < p.size(); i++) {
			if (p[i] > max) {
				max = p[i];
				imax = i;
			}
		}
		for (int j = imax - (max - 1); j <= imax + (max - 1); j++) {
			if (str[j] != '#')
				result += str[j];
		}
		return result;
	}
	void manacher(string& str, vector<int>& p) {
		int len = str.length();
		int id = 0;  // id 为已知的 {右边界最大} 的回文子串的中心
		int mx = 1;  //mx则为id+P[id]，也就是这个子串的右边界
		for (int i = 1; i < len - 2; i++) {
			if (mx > i)
				p[i] = min(p[2 * id - i], mx - i);  //如果mx>i，则分两种情况
			else
				p[i] = 1;                   //如果mx<i，则无法用之前的p来计算，先置1
			for (; str[i + p[i]] == str[i - p[i]]; p[i]++);  //统计i对应的p[i]
			if (mx < i + p[i]) {
				mx = i + p[i]; //更新右边界
				id = i;      //更新id
			}
		}
	}
};
```

方法2：暴力法 n*(n/2)(n2)  时间复杂度O(N^3)

方法3: 动态规划 O(N^2)

## KMP算法-实现 strStr() 函数（leetcode-28）

KMP算法重点是求next数组：

[KMP经典参考文章](https://blog.csdn.net/v_JULY_v/article/details/7041827)

时间复杂度O(M+N)O(M+N)

```c++
class Solution {
public:
    vector<int> getnext(string str)
        {
            int len=str.size();
            vector<int> next;
            next.push_back(-1);//next数组初值为-1
            int j=0,k=-1;
            while(j<len-1)
            {
                if(k==-1||str[j]==str[k])//str[j]后缀 str[k]前缀
                {
                    j++;
                    k++;
                    next.push_back(k);
                }
                else
                {
                    k=next[k];
                }
            }
            return next;
        }
    int strStr(string haystack, string needle) {
        if(needle.empty())
            return 0;
        
        int i=0;//源串
        int j=0;//子串
        int len1=haystack.size();
        int len2=needle.size();
        vector<int> next;
        next=getnext(needle);
        while((i<len1)&&(j<len2))
        {
            if((j==-1)||(haystack[i]==needle[j]))
            {
                i++;
                j++;
            }
            else
            {
                j=next[j];//获取下一次匹配的位置
            }
        }
        if(j==len2)
            return i-j;
        
        return -1;
    }
};
```

库函数解法：

```c++
class Solution {
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(needle.empty())
            return 0;
        int pos=haystack.find(needle);
        return pos;
    }
};
```



# 剑指offer目录

| 题目                                                         | 难度 |                                                              |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| [15.二进制中1的个数.note](note://2FE9426DFA26451590280DDE2FF5D995) | ☆    | 要考虑到负数这种情况，右移数还是左移相与的数？               |
| [38.字符串的排列.note](note://0F2C9A08084941ACABE64F2321602B23) | ☆☆   | 采用递归回溯的方式实现，要考虑字符的重复，输出结果是否为字典序。 |
| [43.1~n整数中1出现的次数.note](note://6433D1219FFA437180403A8BBF8FB764) | ☆☆☆  | 设定整数点（如1、10、100等等）作为位置点i，**求每个位置点为1时的数有多少个，再算出所有情况**    //**根据设定的整数位置，对n进行分割，分为两部分，高位n/i，低位n%i** |
| [把数组排成最小的数.note](note://00DEAC2A377142BDAAECF1A29FDD4F7D) | ☆☆   | 要会用数字转字符串函数 to_string(number1),会写sort()的cmp函数。 |
| [49.丑数.note](note://0E29AFB8ABE446BB98834AFFFF1CDCE1)      | ☆☆☆  | 一个丑数的因子只有2,3,5，那么丑数p = 2 ^ x * 3 ^ y * 5 ^ z   |
| [50.第一次只出现一次的字符.note](note://7B71AF3F8E6341F6A7B00CEF6B8B1C82) | ☆☆   | 采用hash思想，用int map[256]统计字符串中字符的个数，然后从头遍历字符串，到map[256]中寻找对应值，如果为1，则返回。 |
| [50(2).字符流中第一个只出现一次的字符.note](note://495347488DBB4A7D8BBF97F7F6219833) | ☆☆   | 和上题类似，只是字符串S和map[256]作为类的成员变量，每次调用insert()函数时，从字符流中添加一个字符到S的结尾，同时，更新map[256]中对应位，findFirst()仍然按照字符串顺序，遍历map数组。 |
| [求1+2+…+n.note](note://1BAB43C17FA84EF69194DDD5CDDECAE3)    | ☆☆   | 采用递归代替循环，用&&代替if条件语句。                       |
| [把字符串转换成整数.note](note://CDEFFF8B180046AA8FD27260647AF6E8) | ☆☆   | (1)字符串指针是否为空，字符串长度是否为0；(2)考虑字符串的正负，正数要考虑带不带正号；(3)确保除了符号位以外所有的字符必须都是0~9之间的几个字符，否则返回0. |
| [19.正则表达式匹配.note](note://3C01B41508FE4CCEA3220DA0E931715D) | ☆☆☆  | 分多种情况考虑，具体分析见链接                               |
| [58(2).左旋转字符串.note](note://660AC5B81F104AB5B54A6CF4A7701F09) | ☆    | 主要熟悉下string类的一些函数的操作[2.16 C++ string类详解.note](note://81AE2E6AD935400C912AE83D10A7DBAF) |
| [5.替换空格.note](note://73104616EC1B4EEE947B96B4CD1EC0FF)   | ☆☆   | 在同一个字符数组中，通过两个指针进行空格和%20的替换。        |
|                                                              |      |                                                              |
|                                                              |      |                                                              |



## 二进制中1的个数

**题目描述**

输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

**知识补充：**

左移运算符m<<n，在左移n位时候，最左边n位丢弃，同时在最右边补上n个0；

m>>n时分两种情况：(1)无符号数，用0填补最左边n位；

（2）有符号数，则右移之后数字的符号位填补最左侧的n位。

如：00001010>>2 = 00000010            10001010>>3 = 11110001

思路分析：

(1)拿到题目可能第一想法，直接右移(注意：右移比除以2效率高很多)，如果输入的是负数，右移后，最左侧补上的是'1',从而造成死循环；

(2)为了避免死循环，我们不右移输入的数字，而是将其与1,2,4,8...相与，实际采用标志位左移的方式，代码如下：

```c++
class Solution {
public:
     int  NumberOf1(int n) {
         int count = 0;
         int flag = 1;
         while(flag){
             if(n&flag)
                 count++;
             flag = flag<<1;
         }
         return count;
     }
};
```

## 字符串的排列

**题目描述**

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

**输入描述:**

输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

思路：实际上本题需要排除重复情况，结果需要字典排序

**思路：**

**(1)将第一个元素与包括自身在内的所有元素交换。**

**(2)那么第一个元素是固定的，转到下一个元素。**

**(3)直到最后一个元素固定。输出。**

{%asset_img 01.png%}

可以参考LeetCode刷题笔记：[2.Permutations(中)全排列.note](note://38033A07A3AD427AA3CA0E8ABE6D757F)

也可以[参考博客:](https://www.cnblogs.com/AndyJee/p/4655485.html)

```c++
class Solution {
public:
    vector<string> Permutation(string str) {
        if(str.length() <= 0)
            return result;
        permu(str,0);
        sort(result.begin(),result.end());
        return result;
    }
    void permu(string str,int begin){
        if(begin == str.length()-1)  //最后一个字符的自身交换可以不考虑，如果考虑就不减1
            result.push_back(str);
        else{
            for(int i = begin;i<str.length();i++){      
                if(begin == i || str[begin] != str[i]){  //排除重复的影响
                    swap(str[begin],str[i]);
                    permu(str,begin+1);
                    swap(str[begin],str[i]);  //采用回溯的方法
                }
            }
        }
    }
    void swap(char& a,char& b){
        char temp = a;
        a=b;
        b=temp;
    }
private:
    vector<string> result;
};
```



## 1~n整数中1出现的次数

**题目描述**

求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。

**思路：**

设定整数点（如1、10、100等等）作为位置点i（对应n的各位、十位、百位等等），分别对每个数位上有多少包含1的点进行分析，**即求每个位置点为1时的数有多少个，再算出所有情况**

​    //**根据设定的整数位置，对n进行分割，分为两部分，高位n/i，低位n%i**

​    //当i表示百位，且百位对应的数>=2,如n=31456,i=100，则a=314,b=56，此时百位为1的次数有a/10+1=32（最高两位0~31），每一次都包含100个连续的点，即共有(a/10+1)*100个点的百位为1

​    //当i表示百位，且百位对应的数为1，如n=31156,i=100，则a=311,b=56，此时百位对应的就是1，则共有(a/10)*100+(b+1)个数

​    //当i表示百位，且百位对应的数为0,如n=31056,i=100，则a=310,b=56，此时百位为1的次数有(a/10)*100个

```c++
class Solution {
public:
    int NumberOf1Between1AndN_Solution(int n)
    {
        int count =0;
        long i=1;
        for(i=1;i<=n;i*=10){
            int a=n/i;
            int b=n%i;
            if(a%10 == 0)
                count = count+(a/10)*i;
            else if(a%10 >= 2)
                count = count+(a/10 + 1)*i;
            else
                count = count+(a/10)*i+b+1;
        }
        return count;
    }
};
```



## 把数组排成最小的数

**题目描述**

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

**解题思路**

可以看成是一个排序问题，在比较两个字符串S1和S2的大小时，应该比较的是S1+S2和S2+S1的大小，如果S1+S2<S2+S1,那么应该把S1排在前面，否则应该把S2排在前面。

```c++
class Solution {
public:
    string PrintMinNumber(vector<int> numbers) {
        int n = numbers.size();
        if(n==0){
            return "";
        }
        sort(numbers.begin(),numbers.end(),cmp);//核心就一个sort()
        string result;
        for(int i=0;i<n;i++){
            result += to_string(numbers[i]);
        }
        return result;
    }
    //如果在类里面定义cmp要设置为静态的，直接放在类外不用这样，可能和sort()的实现有关
    static bool cmp(int a,int b){
        string A = to_string(a)+to_string(b);
        string B = to_string(b)+to_string(a);
        return A < B;
    }
};
```

## 丑数

**题目描述**

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

**思路：**

首先从丑数的定义我们知道，一个丑数的因子只有2,3,5，那么丑数p = 2 ^ x * 3 ^ y * 5 ^ z，换句话说**一个丑数一定由另一个丑数乘以2或者乘以3或者乘以5得到**，那么我们从1开始乘以2,3,5，就得到2,3,5三个丑数，在从这三个丑数出发乘以2,3,5就得到4，6,10,6，9,15,10,15,25九个丑数，我们发现这种方法会得到重复的丑数，而且我们题目要求第N个丑数，这样的方法得到的丑数也是无序的。那么我们可以维护三个队列：

（1）丑数数组： 1

乘以2的队列：2

乘以3的队列：3

乘以5的队列：5

选择三个队列头最小的数2加入丑数数组，同时将该最小的数乘以2,3,5放入三个队列；

（2）丑数数组：1,2

乘以2的队列：4

乘以3的队列：3，6

乘以5的队列：5，10

选择三个队列头最小的数3加入丑数数组，同时将该最小的数乘以2,3,5放入三个队列；

（3）丑数数组：1,2,3

乘以2的队列：4,6

乘以3的队列：6,9

乘以5的队列：5,10,15

选择三个队列头里最小的数4加入丑数数组，同时将该最小的数乘以2,3,5放入三个队列；

（4）丑数数组：1,2,3,4

乘以2的队列：6，8

乘以3的队列：6,9,12

乘以5的队列：5,10,15,20

选择三个队列头里最小的数5加入丑数数组，同时将该最小的数乘以2,3,5放入三个队列；

（5）丑数数组：1,2,3,4,5

乘以2的队列：6,8,10，

乘以3的队列：6,9,12,15

乘以5的队列：10,15,20,25

选择三个队列头里最小的数6加入丑数数组，但我们发现，有两个队列头都为6，所以我们弹出两个队列头，同时将12,18,30放入三个队列；

……………………

疑问：

1.为什么分三个队列？

丑数数组里的数一定是有序的，因为我们是从丑数数组里的数乘以2,3,5选出的最小数，一定比以前未乘以2,3,5大，同时对于三个队列内部，按先后顺序乘以2,3,5分别放入，所以同一个队列内部也是有序的；

2.为什么比较三个队列头部最小的数放入丑数数组？

因为三个队列是有序的，所以取出三个头中最小的，等同于找到了三个队列所有数中最小的。

实现思路：

我们没有必要维护三个队列，只需要记录三个指针显示到达哪一步；“|”表示指针,arr表示丑数数组；

（1）1

|2

|3

|5

目前指针指向0,0,0，队列头arr[0] * 2 = 2,  arr[0] * 3 = 3,  arr[0] * 5 = 5

（2）1 2

2 |4

|3 6

|5 10

目前指针指向1,0,0，队列头arr[1] * 2 = 4,  arr[0] * 3 = 3, arr[0] * 5 = 5

（3）1 2 3

2| 4 6

3 |6 9

|5 10 15

目前指针指向1,1,0，队列头arr[1] * 2 = 4,  arr[1] * 3 = 6, arr[0] * 5 = 5

```c++
class Solution {
public:
    int GetUglyNumber_Solution(int index) {
        if(index < 7)
            return index;
        vector<int> res(index);
        res[0] =1;
        int p2=0,p3=0,p5=0;
        for(int i=1;i<index;i++){
            res[i]=min_num(res[p2]*2,res[p3]*3,res[p5]*5);
            if(res[i] == res[p2]*2)
                p2++;
            if(res[i] == res[p3]*3)
                p3++;
            if(res[i] == res[p5]*5)
                p5++;
        }
        return res[index -1];
    }
    int min_num(int a,int b,int c){
        a = a < b?a:b;
        a = a < c?a:c;
        return a;
    }
};
```

**我所遇到过的错误分析：**

**在下面代码中，如果用if-else if-else，则会产生重复问题，比如产生两个6,6的重复。   所以一定要用if()....if().....if().....**       

```c++
class Solution {
public:
	int GetUglyNumber_Solution(int index) {
		if (index < 7)
			return index;
		vector<int> result(index);
		result[0] = 1;
		int p2 = 0;
		int p3 = 0;
		int p5 = 0;
		for (int i = 1; i < index; i++) {
			result[i] = find_min(result[p2] * 2, result[p3] * 3, result[p5] * 5);
			if (result[i] == result[p2] * 2)
				p2++;
			else if (result[i] == result[p3] * 3)
				p3++;
			else
				p5++;
		}
		return result[index - 1];
	}
	int find_min(int a, int b, int c) {
		a = a < b ? a : b;
		a = a < c ? a : c;
		return a;
	}
};
```

## 第一次只出现一次的字符

**题目描述**

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

**思路：**

用STL中的map或者直接构件一个哈希表，由于本题只需要一个简单的哈希表，因此考虑实现一个简单的哈希表。

**每个字母根据其ASCII码值作为数组的下标对应数组的一个数字，而数组中存储的是每个字符出现的次数。这样我们就创建了一个大小为256、以字符ASCII码为键值的哈希表。**

```c++
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        int map[256] = {0};
        for(auto c : str)
            map[c]++;
        for(int i = 0; i < str.length(); i++)
        {
            if(map[str[i]] == 1)
                return i;
        }
        return -1;
    }
};
```

## 字符流中第一个只出现一次的字符

**题目描述**

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

**输出描述:**

如果当前字符流没有存在出现一次的字符，返回#字符。

**思路：**

类似于上一题

```c++
class Solution
{
public:
  //Insert one char from stringstream
    void Insert(char ch)
    {
         s+=ch;
        hash[ch]++;
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
        for(int i=0;i<s.length();i++){
            if(hash[s[i]] == 1)
                return s[i];
        }
        return '#';
    }
private:
    string s;
    char hash[256]={0};
};
```

## 求1+2+…+n

**题目：**

求1+2+…+n，要求不能使用乘除法、for、while、ifelse、switch、case等关键字及条件判断语句（A？B:C）。

**思路：**采用递归，用&&代替if判断

```c++
class Solution {
public:
    int Sum_Solution(int n) {
        int ans = n;
        ans&&(ans += Sum_Solution(n-1));
        return ans;
    }
};
```

## 把字符串转换成整数

题目描述：

将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。

思路：本题主要考察思维严密性，注意以下几点：

**(1)字符串指针是否为空，字符串长度是否为0；**

**(2)考虑字符串的正负，特别是正数要考虑带不带正号；**

**(3)确保除了符号位以外所有的字符必须都是0~9之间的几个字符，否则返回0.**

```c++
class Solution {
public:
    int StrToInt(string str) {
        int len = str.length();
        if(len == 0)
            return 0;
        int s = (str[0] == '-' ? -1:1); //记录符号正负
        int i = ((str[0] == '-')||(str[0] == '+')?1:0); //看字符串前有符号没有
        long long result = 0;
        for(;i<len;i++){
            if(str[i]>='0' && str[i]<='9')  //只有每一个单个字符都在0~9之间才合理，否则返回0
                result = result*10 + str[i]-'0';
            else
                return 0;
        }
        return result*s;  //乘以符号位
    }
};
```

## 正则表达式匹配

**题目描述**

请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

**思路：**

解这题需要把题意仔细研究清楚其多种情况。

首先通过模式串是否到结尾作为分类标准，分为两类

【1】模式串到结尾了

这种情况根据字符串是否到结尾了又分为两类：

(1)如果字符串和模式串都到了结尾，则返回true

(2)如果模式串到尾了，字符串还没到尾，肯定匹配失败,返回false

【2】模式串未到结尾

根据模式串当前字符的下一个字符是否为‘*’分为2类：

(1)若不为*，该种情况比较简单，比较字符串和模式串，分为当前字符匹配成功与失败2种；

(2)若为*，该种情况相对复杂，也是比较字符串和模式串，分为当前字符匹配成功与失败2种；

a.若匹配成功，则再分为字符串指针向后移动一位和不考虑模式串此次的成功匹配(考虑模式串出现多次连续与字符串当前位匹配成功的情况)字符串指针不向后移动两种情况，其中字符串指针向后移动一位的情况，又分为模式串指针保持在原位置和向后移动两位。具体如下：

```c++
 return matchCore(str+1,pattern+2)||matchCore(str+1,pattern)||matchCore(str,pattern+2); 
```

b.若匹配不成功，则，字符串指针保持原位，模式串指针向后移动两位。

   为了更加直观说明，各种分类情况如下脑图：

{%asset_img 02.png%}

```c++
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        if(str == NULL || pattern == NULL)
            return false;
        return matchCore(str,pattern);
    }
    bool matchCore(char* str, char* pattern){
        //如果字符串和模式串都到了结尾，则返回true
        if(*str == '\0' && *pattern == '\0')
            return true;
        //如果模式串到尾了，字符串还没到尾，肯定匹配失败
        if(*str != '\0' && *pattern == '\0')
            return false;
///!!!!!!!!!上方为模式串到结尾了，下方是模式串未到结尾的所有情况！！！/////
        if(*(pattern+1) == '*'){  //剩下情况以模式串下一个字符是否为*作为分类标准，分两类
            //如果当前字符串与模式串匹配上了则分为两种情况：【1】字符串和模式串相等，【2】模式串是‘.’，
            //且字符串没有到结尾，则继续匹配，字符串指针向后移1位，模式串指针保持原位或者向后2位
            //此外还要额外考虑一种情况，如字符串abc  模式串 a*a*bc,即模式串可以匹配成功，但是模式串
            //放弃前两位a*，用后面的字符与字符串去匹配，这种情况极易忽略，重点！！！
            if(*str == *pattern || (*str != '\0'&&*pattern == '.'))
                return matchCore(str+1,pattern+2)||matchCore(str+1,pattern)||matchCore(str,pattern+2);
            else  //如果字符串和模式串没配上，继续配，只能认为模式串*字符前一位字符出现0个
                return matchCore(str,pattern+2);
        }
        else{
            if(*str == *pattern || (*str != '\0'&&*pattern == '.'))
                return matchCore(str+1,pattern+1);
            else
                return false;
        }
    }
};
```

## 左旋转字符串

本题重点了解字符串string的用法：[2.16 C++ string类详解.note](note://81AE2E6AD935400C912AE83D10A7DBAF)

对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。

```c++
class Solution {
public:
    string LeftRotateString(string str, int n) {
        int len = str.length();
        if(len < n)
            return str;
        string temp = str.substr(n);
        string result = str.substr(0,n);
        result = temp + result;
        return result;
    }
};
```

## 替换空格

**题目描述**

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为4We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

**思路：**

{%asset_img 03.png%}

```c++
class Solution {
public:
	void replaceSpace(char *str,int length) {
        //先判断是否为空字符数组
        if(str == NULL || length <= 0)
            return;
        
    //计算字符数组的实际长度(带\0)/////begin////
        int trueLength = 0;
        char* pTemp = str;
        int count = 0;
        for(int i=0;str[i] != '\0';i++){
            trueLength++;
            if(str[i] == ' ')
                count++;
        }
        trueLength++;
        int newlength = count*2+trueLength;
    //计算字符数组的实际长度(带\0)/////end/////
        
        if(newlength > length)//如果替换空格后所需的字符数组长度小于length,则返回
            return;
        //进行替换////begin/////
        int indexOriginal = trueLength-1;
        int indexNew = newlength-1;
        while(indexOriginal >= 0 && indexNew >= 0){
            if(str[indexOriginal] == ' '){
                str[indexNew--] = '0';
                str[indexNew--] = '2';
                str[indexNew--] = '%';
            }
            else{
                str[indexNew--] = str[indexOriginal];
            }
            indexOriginal--;
        }
          //进行替换////end/////
	}
};
```

