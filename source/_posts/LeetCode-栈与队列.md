---
title: '栈与队列'
date: 2019-06-23 13:52:09
tags:
	- 栈与队列刷题
categories: 
	- 数据结构与算法
	- 算法刷题
---

## 总结目录

| 题目                       | 难度 | 收获与总结                                                   |
| -------------------------- | ---- | ------------------------------------------------------------ |
| 1.用队列实现栈             | 易   | 只是push()函数需要修改，每次添加一个数的时候，把队列中原有的数按顺序出队列再进队列。 |
| 2.用栈实现队列             | 易   | 只有push()函数需要改，**用一个临时栈**，每次添加一个数的时候，将数据栈中数按照顺序放临时栈中，将要push()的数加入数据栈中后，再将临时栈中的数据按照顺序放回数据栈。 |
| 3.实现含有getMin()函数的栈 | 易   | 需要改push()和pop()函数，使用两个栈，一个正常放数据，一个记录每个push()和pop()操作对应栈中最小的数。 |
| 4.找出数组中第K大的数      | 易   | 利用STL中priority_queue(PriorityQueue队列,是基于最小堆原理实现),也可以用STL中的sort()函数先排序。 |

## Implement(实现) Stack using Queues(易)

225. Implement Stack using Queues

Implement the following operations of a stack using queues.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- empty() -- Return whether the stack is empty.

**Example:**

```c++
MyStack stack = new MyStack();

stack.push(1);
stack.push(2);  
stack.top();   // returns 2
stack.pop();   // returns 2
stack.empty(); // returns false
```

**Notes:**

- You must use *only* standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.
- Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
- You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).

```c++
class MyStack {
public:
    /** Initialize your data structure here. */
    MyStack() {
         
    }
     
    /** Push element x onto stack. */
    void push(int x) {
        mQueue.push(x);
        if(mQueue.size() > 1){
            for(int i = 0;i<mQueue.size() -1 ;i++){
                mQueue.push(mQueue.front());
                mQueue.pop();
            }
        }
    }
     
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int a = mQueue.front();
        mQueue.pop();
        return a;
    }
     
    /** Get the top element. */
    int top() {
        return mQueue.front();
    }
     
    /** Returns whether the stack is empty. */
    bool empty() {
        return mQueue.empty();
    }
     
private:
    queue<int> mQueue;
};
 
/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * bool param_4 = obj.empty();
 */
```

## Implement Queue using Stacks(易)

\232. Implement Queue using Stacks

Implement the following operations of a queue using stacks.

- push(x) -- Push element x to the back of queue.
- pop() -- Removes the element from in front of queue.
- peek() -- Get the front element.
- empty() -- Return whether the queue is empty.

**Example:**

```c++
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

**Notes:**

- You must use *only* standard operations of a stack -- which means only push to top, peek/pop from top, size, and is emptyoperations are valid.
- Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
- You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

class MyQueue {
public:
    /** Initialize your data structure here. */
    MyQueue() {

    }
     
    /** Push element x to the back of queue. */
    void push(int x) {
        std::stacktempStack;
        while(!mStack.empty()){
            tempStack.push(mStack.top());
            mStack.pop();
        }
        mStack.push(x);
        while(!tempStack.empty()){
            mStack.push(tempStack.top());
            tempStack.pop();
        }
    }
     
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        int a = mStack.top();
        mStack.pop();
        return a;
    }
     
    /** Get the front element. */
    int peek() {
        return mStack.top();
    }
     
    /** Returns whether the queue is empty. */
    bool empty() {
        return mStack.empty();
    }
private:
    stackmStack;
    stacktempStack;
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * bool param_4 = obj.empty();
 */

{%asset_img 01.png%}

{%asset_img 02.png%}

{%asset_img 03.png%}

{%asset_img 04.png%}

## Min Stack(易)

{%asset_img 05.png%}

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

**Example:**

```c++
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

**题目思路：用两个栈，一个正常地存储数据，另一个存储每一步的最小值。**

{%asset_img 06.png%}

{%asset_img 07.png%}

{%asset_img 08.png%}

{%asset_img 09.png%}

```c++
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
        
    }
    
    void push(int x) {
        mStack.push(x);
        if(minStack.empty()){
            minStack.push(x);
        }
        else{
            if(x < minStack.top()){
                minStack.push(x);
            }
            else{
                minStack.push(minStack.top());
            }
        }        
    }
    
    void pop() {
        mStack.pop();
        minStack.pop();
    }
    
    int top() {
        return mStack.top();
    }
    
    int getMin() {
        return minStack.top();
    }
private:
    std::stack<int> mStack;
    std::stack<int> minStack;
    
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

## Kth Largest Element in an Array（易）

Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example 1:**

```c++
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Example 2:**

```c++
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

**Note:** 

You may assume k is always valid, 1 ≤ k ≤ array's length.

**思路：直接用STL中的priority_queue（**PriorityQueue队列,是基于最小堆原理实现**）**

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int>pQueue;
        for(int i = 0;i < nums.size();i++){
            pQueue.push(nums.at(i));
        }
        for(int i = 0;i < k - 1;i++){
            pQueue.pop();
        }
        return pQueue.top();
    }
};
```

