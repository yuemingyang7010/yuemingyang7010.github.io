---
title: '2-QT中互斥与信号量'
date: 2019-04-3 13:52:09
tags:
	- C++基础
	- 并发编程
categories: 
	- C++基础
	- 并发编程
---

## 1.QMutex类

QMutex的目的是保护对象、数据结构或代码段，以便一次只有一个线程可以访问它(这类似于Java synchronized关键字)。通常最好将互斥锁与QMutexLocker一起使用，因为这样可以很容易地确保一致地执行锁定和解锁。

```c++
int number = 6;

void method1()
{
    number *= 5;
    number /= 4;
}

void method2()
{
    number *= 3;
    number /= 2;
}
```

缺陷：如中代码中间退出，比如有多个if条件里面有return，则导致无法ulock,造成死锁。

解决方法：在每个return之前注意添加unlock，或者用更加方便的

## 2.QMutexLocker类

使用方法：

（1）先定义一个QMutex类的变量

（2）在需要上锁的地方定义一个QMutexLocker类的变量

```c++
#include <QMutex>
QMutex qMutex;
 
//线程槽函数
void WorkThread::startThreadSlot()
{
     QMutexLocker m_lock(&qMutex);
    while(true)
    {
        if(isStop)
            return;
        for(int n=0;n<10;n++)
            qDebug()<<n<<n<<n<<n<<n<<n<<n<<n;
         QThread::sleep(1);
    }
 
}
```

## 3.信号量QSemaphore类

信号量可以理解为对互斥量功能的扩展，互斥量只能锁定一次而信号量可以获取多次，它可以用来保存一定数量的同种资源。

**以下以单个生产者和单个消费者模型举例：**

```c++
#include <QCoreApplication>
#include <QSemaphore>
#include <QThread>
#include <stdio.h>
 
const int DataSize=1000;
const int BufferSize=80;
int buffer[BufferSize];//缓存数据
QSemaphore freeBytes(BufferSize);//freeBytes信号量控制可被生产者填充的缓冲部分
QSemaphore usedBytes(0);//usedBytes信号量控制可被消费者读取的缓冲区部分
 
class Producer : public QThread
{
public:
    Producer();
    void run();
};
 
Producer::Producer()
{
}
 
void Producer::run()
{
    for(int i=0;i<DataSize;i++)
    {
       freeBytes.acquire();//生产者线程首先获取一个空闲单元，如果此时缓冲区培被消费者尚未读取的数据填满，对
       //此函数的调用就会阻塞，直到消费者读取了这些数据为止
       buffer[i%BufferSize]=(i%BufferSize);//一旦生成者获取了某个空闲单元，就使当前的缓存单元序号填写这个缓冲区单元
       usedBytes.release();//调用该函数将可用资源加1，表示消费者此时可以读取这个刚刚填写的单元
    }
}
//Consumer继承QThread类
class Consumer : public QThread
{
public:
    Consumer();
    void run();
};
 
Consumer::Consumer()
{
}
 
void Consumer::run()
{
    for(int i=0;i<DataSize;i++)
    {
        usedBytes.acquire();//消费者线程首先获取一个可被读取的单元，如果缓冲区中没有包含任何可以读取的数据，对此函数的调用就会阻塞，直到生产者生产了一些数据为止
        fprintf(stderr,"%d",buffer[i%BufferSize]);//一旦消费者读取了这个单元，会将这个单元的内容打印出来
        if(i%16==0&&i!=0)
            fprintf(stderr,"\n");
 
        freeBytes.release();//调用该函数使得这个单元变为空闲，已被生产者下次填充
    }
    fprintf(stderr,"\n");
}
 
int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);
 
    Producer producer;
    Consumer consumer;
 
    producer.start();
    consumer.start();
 
    producer.wait();
    consumer.wait();
 
    return a.exec();
}
```



互斥量可以锁定一次，而信号量可以在设置上限大小的情况下，获取多次，可以用来保护一定数量的同种资源。在使用acquire函数跨线程获取n个资源。release(n)可以释放n个资源。当没有足够的资源时，调用者将被阻塞直到有足够的资源可用。

**一个生产者与多个消费者：**

```c++
#include <QCoreApplication>
#include <QSemaphore>
#include <QThread>
#include <QMutex>
#include <iostream>
 
using namespace std;
QSemaphore vacancy(10);      //资源上限
QSemaphore produce(0);       //产品数量
QMutex mutex;                //互斥锁
int buffer[5];               //缓冲区可以放5个产品
int numtaken=30;
int takei=0;
 
class Producer:public QThread
{
    public:
    void run();
};
//生产者线程
void Producer::run()
{
    for(int i=0;i<30;i++)    //生产30次产品
    {
        vacancy.acquire();
        buffer[i%5]=i;
        printf("produced %d\n",i);
        produce.release();
    }
}
class Consumer:public QThread
{
    public:
    void run();
};
void Consumer::run()         //消费者线程
{
    while(numtaken>1)
    {
        produce.acquire();
        mutex.lock();        //从缓冲区取出一个产品,多个消费者,不能同时取出,故用了互斥锁
        printf("thread %ul take %d from buffer[%d] \n",currentThreadId(),buffer[takei%5],takei%5);
        takei++;
        numtaken--;
        mutex.unlock();
        vacancy.release();
    }
}
int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);
    Producer producer;
    Consumer consumerA;
    Consumer consumerB;
    Consumer consumerC;
    producer.start();
    consumerA.start();
    consumerB.start();
    consumerC.start();
    producer.wait();
    consumerA.wait();
    consumerB.wait();
    consumerC.wait();
    return 0;
}
```

执行结果如图1所示：

{%asset_img 01.png%}