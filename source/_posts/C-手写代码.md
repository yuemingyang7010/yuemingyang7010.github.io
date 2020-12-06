---
title: 'C++手写代码'
date: 2019-04-2 13:52:09
tags:
	- C++基础
	- 手写代码
categories: 
	- C++基础
---

### 1.手写简单的string类

String.h

```c++
class String
{
public:
	String(const char *str = NULL);// 普通构造函数  
	String(const String &other);// 拷贝构造函数  
	~String(void);// 析构函数  
	String & operator = (const String &other);// 赋值函数  
private:
	char *m_data;// 用于保存字符串  
};
```

String.cpp

```c++
#include "String.h"
//普通构造函数  
String::String(const char *str)
{
	if (str == NULL)
	{
		m_data = new char[1];// 得分点：对空字符串自动申请存放结束标志'\0'的，加分点：对m_data加NULL判断  
		*m_data = '\0';
	}
	else
	{
		int length = strlen(str);
		m_data = new char[length + 1];// 若能加 NULL 判断则更好
		strcpy(m_data, str);
	}
}

// String的析构函数  
String::~String(void)
{
	delete[] m_data; // 或delete m_data;  
}

//拷贝构造函数  
String::String(const String &other)// 得分点：输入参数为const型  
{
	int length = strlen(other.m_data);
	m_data = new char[length + 1];// 若能加 NULL 判断则更好  
	strcpy(m_data, other.m_data);
}

//赋值函数  
String & String::operator = (const String &other) // 得分点：输入参数为const型  
{
	if (this == &other)//得分点：检查自赋值  
		return *this;
	if (m_data)
		delete[] m_data;//得分点：释放原有的内存资源  
	int length = strlen(other.m_data);
	m_data = new char[length + 1];//加分点：对m_data加NULL判断  
	strcpy(m_data, other.m_data);
	return *this;//得分点：返回本对象的引用    
}
```

测试结果：

```c++
#include <stdio.h>
#include "String.h"
using namespace std;
int main(){
//【赋值方法1】
	String str1("ab");
//【赋值方法2】
	String str2;
	str2 = "abc";
//【赋值方法3】
	String str3 = str2;
//【赋值方法4】
	String str4(str3);
	printf("%s,%s,%s,%s", str1,str2,str3,str4);
	return 0;
}
```





