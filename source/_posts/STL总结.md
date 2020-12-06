---
title: STL总结
date: 2019-07-30 20:26:49
tags:
	- STL
categories: 
	- 数据结构与算法
	- STL总结
---

## string

字符串类

```c++
#include <string>

//1：string对象的定义和初始化以及读写

string s1;        //默认构造函数，s1为空串
string s2(s1);    //将s2初始化为s1的一个副本
string s3("valuee");   //将s3初始化一个字符串面值副本
string s4(n,'c');      //将s4 初始化为字符'c'的n个副本

cin>>s5;               //读取有效字符到遇到空格
getline(cin,s6);      //读取字符到遇到换行，空格可读入，知道‘\n’结束（练习在下一个代码中），
getline(cin,s7,'a'); //一个直到‘a’结束，其中任何字符包括'\n'都能够读入

//2：string对象中一些函数

/*------插入函数------包括迭代器操作和下标操作，下标操作更灵活*/
s.insert( it , p );  //把字符串p插入到it的位置
s.insert(p,n,t)；   //迭代器p元素之前插入n个t的副本
s.insert(p,b,e);     //迭代器p元素之前插入迭代器b到e之间的所有元素
s.insert(p,s2,poe2,len); //在下标p之前插入s2下标从poe2开始长度为len的元素
s.insert(pos,cp,len);  //下标pos之前插入cp数组的前len个元素。

/*----------替换函数-------------------*/
s.substr(i,j)   截取s串中从i到j的子串  //string::npos  判断字符串是否结束
s.replace ( 3 , 3 , " good " ) ;   //从第三个起连续三个替换为good
s.assign(b,e);  //用迭代器b到e范围内的元素替换s
s.assign(n,t)；  //用n个t的副本替换s
a.assign(s1,pos2,len);   //从s1的下标pos2开始连续替换len个。

/*-----------------------删除函数-----------------------------*/
s.erase( 3 )||s.erase ( 0 , 4 ) ;  //删除第四个元素或第一到第五个元素

/*----------------------其他函数-----------------------------*/
s.find ( " cat " ) ;  //超找第一个出现的字符串”cat“，返回其下标值，查不到返回 4294967295，也可查找字符；
s.append(args); //将args接到s的后面
s.compare ( " good " ) ;  //s与”good“比较相等返回0，比"good"大返回1，小则返回-1；
reverse ( s.begin(), s.end () );  //反向排序函数，即字符串反转函数
```



## vector

功能：动态数组,**可随机存取**

底层实现 ：
首先开辟一定大小的数组 随着元素的增加，如果空间不够之后，以原空间大小的2倍重新开辟一块空间， 将就空间的元素挪到新空间上 在继续添加元素，一直遵循每次扩容大小是原空间大小的2倍。

相关用法：

```c++
#include<vector>

//【1】初始化
vector<int> vec;        //声明一个int型向量
vector<int> vec(10);    //声明一个初始大小为10的int向量
vector<int> vec(10, 1); //声明一个初始大小为10且值都是1的向量

vector<int> vec1(vec);  //调用拷贝构造函数

vector<int> tmp(vec.begin(), vec.begin() + 3);  //用向量vec的第0个到第2个值初始化tmp
int array[5] = {1, 2, 3, 4, 5};   
vector<int> vec(array, array + 5);      //将arr数组的元素用于初始化vec向量
//！！！！易错！！！！末尾指针都是指结束元素的下一个元素和vec.end()指针统一

vector<int> vec(&arr[1], &arr[4]); //将arr[1]~arr[4]范围内的元素作为vec的初始值


//【2】大小操作
vec.empty()    //判断是否为空
vec.size()     //输出实际大小
vec.max_size()  //输出最大容量
vec.resize()    //重新定义大小,保留适当的容量

//【3】插入元素
vec.push_back();            //末尾添加元素
vec.insert(vec.begin()+i,a); //任意位置插入元素 在第i+1个元素前面插入a

//【4】删除元素
vec.pop_back();    	 //末尾删除元素 
vec.clear();	     //清空向量元素
vec.erase (vec.begin()+5);                 // erase the 6th element
vec.erase (vec.begin(),vec.begin()+3); // erase the first 3 elements:
  

//【7】迭代器
vec.begin();		//起始指针：
vec.end(); 			//指向最后一个元素的下一个位置

vec.cbegin(); 		//不能通过这个指针来修改所指的内容，但可以通过其他方式修改的。
vec.cend();			//指向常量的末尾指针
vec.rbegin()		//反向迭代器头
vec.rend()			//反向迭代器尾
vec.crbegin()
vec.crend()

//【8】元素访问
vec[1]; 			//下标访问，并不会检查是否越界
vec.at(1); 			//at会检查是否越界，会抛出out of range异常
vec.front();        //访问第一个元素 
vec.back();         //访问最后一个元素

vec.swap(vec2); 		 //交换两个向量的元素
swap(vec,vec2);
```

## list

实际上,list容器就是一个双向链表,可以高效地进行插入删除元素。

```c++
#include <list>
//构造函数
list<int> first;                                // empty list of ints
list<int> second (4,100);                       // four ints with value 100
list<int> third (second.begin(),second.end());  // iterating through second
list<int> fourth (third);                       // a copy of third

//插入元素
a.insert(a.begin(),100);  //在a的开始位置（即头部）插入100
a.insert(a.begin(),2, 100);   //在a的开始位置插入2个100
a.insert(a.begin(),b.begin(), b.end());//在a的开始位置插入b从开始到结束的所有位置的元素
list.push_back(x)  //在链表尾插入元素
list.push_front(x) //在链表头插入元素

//迭代器
list.begin()    //获得指向链表尾的指针
list.end()      //获得指向链表尾（尾的下一个元素）的指针
list.rbegin()	//获得反向链表的头指针
list.rend()		//获得反向链表的尾（尾的下一个元素）指针


//大小判断
list.empty()      //判断是否为空，为空，返回true
list.size()		  //返回list的实际大小
list.max_size()   //返回list的最大容量

//获得元素
list.front()      //获得头元素
list.back()       //获得尾元素

//删除操作
a.erase(a.begin()); 		 //将a的第一个元素删除
a.erase(a.begin(),a.end());  //将a的从begin()到end()之间的元素删除。
//必须保证不为空
list.pop_back()   //删除尾元素
list.pop_front()  //删除头元素
list<int> a {6,7,8,9,7,10}; a.remove(7);   //删除指定元素


//其他操作
list.assign(n,value)  //list将被改为n个值为value的元素
list.resize(x)   //将链表改为长为x   超出的部分将被删除
swap(a,b)   //交换a,b链表的值
list.reverse()   //将链表倒置
a.merge(b)   //将链表b添加到链表a的后面

//补充以下迭代器的使用
//以下是四种迭代器的遍历操作（如果使用C11标准，可以直接使用auto）
    list<int>::iterator cur1=li.begin();
    for(;cur1!=li.end();cur1++){
        cout<<*cur1<<" ";
    }
    cout<<endl;
    
    list<int>::const_iterator cur2=li.cbegin();
    for(;cur2!=li.cend();cur2++){
        cout<<*cur2<<" ";
    }
    cout<<endl;

//C11标准
    auto cur=li.begin();
    for(;cur!=li.end();cur++){
        cout<<*cur<<" ";
    }
```

## stack

栈 （后进先出）

```c++
#include<stack>

stack<int> sta;
//大小操作
sta.empty()
sta.size()

sta.push(x)  //将x加入到栈顶
sta.pop()	 //将栈顶元素弹出
sta.top()	 //返回栈顶元素

//C11
sta.swap(sta1) //交换栈sta和sta1中的元素
sta.emplace(x) //将x放入到栈顶

//栈的清空
while(!sta.empty()) sta.pop();
```

## queue

队列 （先进先出）

```c++
#include<queue>

queue<int> que;
que.size()   //返回队列中元素的个数
que.empty()  //如果为空，返回true

que.front()  //返回第一个元素（即队首元素）
que.back()   //返回队尾元素

que.pop()    //删除第一个元素
que.push(x)  //在队尾加入一个元素

que.swap(que1) 	//交换que和que1的元素
que.emplace(x)  //向队首加入元素

//queue不提供清空操作，一般手动实现
while(!que.empty()) que.pop();
```

## priority_queue

优先队列 (STL中的堆)

```c++
#include<queue>
/*
*  模板原型：
*  priority_queue<Type,Container,Functional>
*/
priority_queue<int> que;     //默认是降序的
priority_queue<int,vector<int>,greater<int>> que1;  //升序队列
priority_queue<int,vector<int>,less<int>> que2;     //降序队列

//对于结构体的比较
class student {
	public:
		string name;
		int score;
		student(string na, int sc):name(na), score(sc) {}
};
struct cmp {
	bool operator() (const student& a, const student& b ) {
		return a.score > b.score;
	}
};
priority_queue<student, vector<student> , cmp> que;   //根据成绩从大到小排序

//具体操作
que.empty();
que.size();

que.push(x);		//加入元素到队尾
que.pop();			//从队首删除元素

que.top();     		//返回队首元素

que.emplace(x)
que.swap(que1)
```

## map

map对于key是随机存取的

```c++
#include<map>
#include <utility>  //使用pair与make_pair要包含头文件#include <utility>
map<int, int> ma;
//必会操作
ma.begin();         //返回指向头部的迭代器
ma.end();           //返回指向末尾的迭代器
ma[i] = A;      //插入 A
mp.insert(make_pair(i,A));  //插入 A
ma.erase(i);    //删除 i
ma.clear();     //删除所有元素
ma.find(i);     //查找 i (若未找到返回 end())
ma.empty();     //如果map为空则返回true
ma.size();      //返回map中元素的个数


ma.swap();    //交换两个map
ma.lower_bound();   //返回 >=给定元素的第一个位置
ma.upper_bound();   //返回 >给定元素的第一个位置
ma.max_size();      //返回可以容纳的最大元素个数
```

## set

和map操作几乎一样，只是插入的为key即为value

```c++
#include<set>
set<int> se;
//必会操作
se.begin();         //返回指向头部的迭代器
se.end();           //返回指向末尾的迭代器
se.insert(A);      //插入 A
se.erase(A);    //删除 A
se.clear();     //删除所有元素
se.find(i);     //查找 i (若未找到返回 end())
se.empty();
se.size();
    
se.swap();    //交换两个map
se.lower_bound();   //返回 >=给定元素的第一个位置
se.upper_bound();   //返回 >给定元素的第一个位置
se.max_size();      //返回可以容纳的最大元素个数
```

还有 hash_table 没有写，hash_table 是兼顾各项，在元素不 “冲突” 的情况下，上面四个可以，而且速度很快。

unordered_set、unordered_multiset、unordered_map、unordered_multimap 都是以 hash_table 作为底层实现的。所以效率要比 RB_tree 作为底层实现的 set、map、multiset、multimap 高，但是 hash_table 的缺点是没有进行排序。

stack、queue 都是以 deque（双端队列）作为底层实现的，效率问题直接看deque就行。