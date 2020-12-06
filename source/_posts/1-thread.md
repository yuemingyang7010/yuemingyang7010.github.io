---
title: '1.thread'
date: 2019-04-2 13:52:09
tags:
	- C++基础
	- 并发编程
categories: 
	- C++基础
	- 并发编程
---

从C++11新标准开始，C++语言本身增加了对多线程的支持，意味着使用C++可实现多线程程序的可移植，跨平台。一个书写良好的程序，必须等所有的子线程执行完毕之后，主线程才能结束。

## 一、C++11创建线程基本方法

### 1.1 使用函数创建线程

#### 1 添加头文件

添加头文件以使用多线程：#include <thread>

#### 2  创建初始函数

定义一个函数，这个函数将会作为我们创建的线程的初始函数：

```c++
void thread_function()
{
	cout << "start executing my thread..." << endl;
}
```

3 在程序中添加线程，以及基本的线程函数

新建一个线程使用：

```c++
thread 线程名称（初始函数） 
```

**join()函数**

用法：线程名称.join()

说明：使用该函数后，主线程阻塞到这里，当子线程执行完毕，这个join()函数就执行完毕，主线程也就可以继续运行了。

**detach()函数**

传统多线程程序，主线程要等待子线程执行完毕，然后自己再退出。

deteach的意思是分离，也就是主线程不必和子线程汇合了，主线程与子线程各自执行。引入detach()是为了避免让主线程逐个等待子线程结束。

**joinable()函数**

判断是否可以成功使用join()或者detach()，返回true则可以，返回false则不可以。

最后的完整程序如下，您可以通过依次运行mythread.join()，mythread.detach()函数查看程序运行结果以加深对这几个函数的理解。

```c++
#include <iostream>
#include <thread>
using namespace std;
 
void thread_function()
{
    for(int i = 0; i < 5; i++)
        cout << "子线程运行" << i+1 << endl;
}
 
int main()
{
    thread mythread(thread_function);        // 传递初始函数作为线程的参数
    if(mythread.joinable())
        mythread.join();                     // 使用join()函数阻塞主线程直至子线程执行完毕
    if(mythread.joinable())
        mythread.detach();                   // 使用detach()函数让子线程和主线程并行运行，主线程也不再等待子线程。
	
    for(int i = 0; i < 5; i++)
        cout << "主线程运行" << i+1 << endl;
 
    return 0;
} 
```

#### 4 运行结果分析

如果只使用join()函数，注释detach()函数，运行结果如下，为顺序执行： 

```c++
子线程运行1
子线程运行2
子线程运行3
子线程运行4
子线程运行5
主线程运行1
主线程运行2
主线程运行3
主线程运行4
主线程运行5
```

​	如果只使用detach()函数，运行结果如下，每次执行的结果均不相同：

```c++
子线程运行主线程运行11

主线程运行2
子线程运行2
子线程运行3
子线程运行4
子线程运行5
主线程运行3
主线程运行4
主线程运行5
```

### 1.2 使用类对象创建线程

创建thread的时候参数不仅可以是一个函数，也可以是一个类的对象。这个类中需要重载运算符()，如下所示：

```c++
#include <iostream>
#include <thread>
using namespace std;
 
class Thread
{
public:
    // 运算符重载 
    void operator()()
    {
        for(int i = 0; i < 5; i++)
        cout << "子线程运行" << i+1 << endl;
    }	
};
 
int main()
{
    Thread t;
    thread mythread(t);        // 传递初始函数作为线程的参数
    if(mythread.joinable())
        mythread.join();       // 使用join()函数阻塞主线程直至子线程执行完毕
    if(mythread.joinable())
        mythread.detach();     // 使用detach()函数让子线程和主线程并行运行，主线程也不再等待子线程。
	
    for(int i = 0; i < 5; i++)
        cout << "主线程运行" << i+1 << endl;
 
    return 0;
} 
```

detach()之后子线程就分离了，运行完了由C++运行时库回收，并且detach()之后就不能join了。

### 1.3 detach()的坑

坑的原因：子线程还没运行完，主线程就运行完了。

子线程参数通过构造临时对象的方式来进行传递，构造临时对象主要有两种情况
（1）传递int这种简单类型参数，建议使用值传递，不要使用引用，防止节外生枝。
（2）如果传递类对象，一律在创建线程的地方就通过隐式转换构建临时对象，然后在函数形参处使用引用（节约资源）。

