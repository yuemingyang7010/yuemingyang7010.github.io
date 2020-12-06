---
title: '剑指offer-链表、队列与栈'
date: 2019-06-20 13:52:09
tags:
	- 剑指offer
categories: 
	- 数据结构与算法
	- 剑指offer
---

## 目录

| 题目                                                         | 难度 |                                                              |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| [6.从尾到头打印链表.note](note://267A264815194320AE99C6456F7F9067) | ☆    | 即链表逆序，如果不能改变原始链表结构，可用栈                 |
| [22.链表中倒数第K个节点.note](note://188647273FFD47D28D4BC73B9689F456) | ☆    | 快慢指针法                                                   |
| [25.合并两个排序的链表.note](note://1EB18F62FA924217AF3E9890E393B48F) | ☆    | 先获取新链表头(两链表最小的头)，然后按照递增方式连接，最后将某一个剩余一段的链表直接接上。 |
| [35.复杂链表的复制.note](note://A4047F5CE1E345E2917C4B71250C1BFB) | ☆☆☆  | 细心！！分三步：1，复制每个节点，插在其后；2，复制每个旧节点的random到新节点；3.拆分节点。 |
| [圆圈中最后剩下的数.note](note://2B3B33D6AFDE4BD98362D2EC0146B083) | ☆☆☆  | 约瑟夫环问题，采用STL中的list构成环形链表。                  |
|                                                              |      |                                                              |



| 题目                                                         | 难度 |                                                            |
| ------------------------------------------------------------ | ---- | ---------------------------------------------------------- |
| [9.用两个栈实现队列(易).note](note://E97F872F78694F578300934A2651FFB5) | ☆    | 模拟队列先进先出，一个栈存数据，另一个栈临时放数据。       |
| [30.包含min函数的栈.note](note://EFF3BEAF7B584C859F553D330A86E4E2) | ☆    | 设置两个栈，一个存放数据，一个存放每一步数据栈中最小的数。 |
| [31.栈的压入、弹出序列.note](note://E815AA4D2EF94E8CA776D8A770604A4F) | ☆☆☆  | 用一个栈来模拟压入弹出操作。                               |
|                                                              |      |                                                            |



## 从尾到头打印链表

**题目描述**

输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

**思路分析：**

其实本题和链表逆序很类似，链表逆序要求改变链表，采用头插法。此题逆序打印链表，一般理解为不改变链表结构，因此采用栈的结构。

```c++
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
#include <stack>
#include <vector>
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        std::stack<ListNode*> nodes;
        vector<int> mVector;
        ListNode* pNodes = head;
        while(pNodes != NULL){
            nodes.push(pNodes);
            pNodes = pNodes->next;
        }
        while(!nodes.empty()){
            pNodes = nodes.top();
            mVector.push_back(pNodes->val);
            nodes.pop();
        }
        return mVector;
    }
};
```

## 链表中倒数第K个节点

输入一个链表，输出该链表中倒数第k个结点。

思路：快慢指针

```c++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        ListNode* fast = pListHead;
        ListNode* low = pListHead;
        while(k > 0){
            if(fast == NULL){
                return NULL;   //防止越界错误！！！要加
            }
            fast = fast->next;
            k--;
        }
        while(fast != NULL){
            fast = fast->next;
            low = low->next;
        }
        return low;
    }
};
```

## 合并两个排序的链表

**题目描述**

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

{%asset_img 01.png%}

非递归代码：

```c++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1 ==NULL)
            return pHead2;
        if(pHead2 == NULL)
            return pHead1;
        ListNode* pre_head = new ListNode(0);
        ListNode* pTemp = pre_head;
        while(pHead1 !=NULL && pHead2 != NULL){
            if(pHead1->val < pHead2->val){
                pTemp->next = pHead1;
                pHead1 = pHead1->next;
            }
            else{
                pTemp->next = pHead2;
                pHead2 = pHead2->next;
            }
            pTemp = pTemp->next;
        }
        //将比较后某一个原始链表剩余部分直接插到新的链表后方
        if(pHead1)
            pTemp->next = pHead1;
        if(pHead2)
            pTemp->next = pHead2;
        ListNode* ans = pre_head->next;
        delete pre_head;
        return ans;
    }
};
```

## 复杂链表的复制

**题目描述**

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

思路：(在不借助辅助空间的情况下，实现O(n)的时间效率)

分为三步走：

{%asset_img 02.png%}

{%asset_img 03.png%}

{%asset_img 04.png%}

```c++
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(pHead == NULL)
            return NULL;
        //第一步：遍历链表，复制每个结点，如复制结点A得到A1，将结点A1插到结点A后面；
        RandomListNode* curNode = pHead;
        while(curNode != NULL){
            RandomListNode* pCloned = new RandomListNode(0);
            pCloned->label = curNode->label;
            pCloned->next = curNode->next;
            pCloned->random = NULL;
            curNode->next = pCloned;
            curNode = pCloned->next;
        }
        //第二步：重新遍历链表，复制老结点的随机指针给新结点，如A1.random = A.random.next;
        curNode = pHead;
        while(curNode != NULL){
            RandomListNode* pCloned = curNode->next;
            if(curNode->random != NULL){  //！！！！很关键的一个判断，不然就出现野指针
                pCloned->random = curNode->random->next;
            }
            curNode = pCloned->next;
        }
        //第三步：拆分链表，将链表拆分为原链表和复制后的链表
        curNode = pHead;
        RandomListNode* pCloneHead = pHead->next;
        while(curNode != NULL){
            RandomListNode* pCloneNode = curNode->next;  //临时存放当前节点的下一个指针
            curNode->next = curNode->next->next;  //原始链表，隔一个节点连接一个
            //复制后链表，隔一个节点连接一个，但是要考虑该节点是最后一个的情况
            pCloneNode->next = pCloneNode->next == NULL?NULL:pCloneNode->next->next;
            curNode = curNode->next;  //移到下一个原始节点
        }
        return pCloneHead;
    }
};
```

## 圆圈中最后剩下的数

圆圈中最后剩下的数

题目：0，1，n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。

**思路：用约瑟夫环的思想**

例如，0、1、2、3、4这5个数字组成一个圆圈（如图6.3所示），从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

{%asset_img 05.png%}

可以用模板库中的std:list来模拟一个环形链表。由于std:：list本身并不是一个环形结构，因此每当迭代器（Iterator）扫描到链表末尾的时候，我们要记得把迭代器移到链表的头部。

```c++
class Solution {
public:
	int LastRemaining_Solution(int n, int m)
	{
		if (n < 1 || m < 1)
			return -1;
		list<int> mlist;
		for (int i = 0; i < n; i++)
			mlist.push_back(i);
		long count = 0;
		auto it = mlist.begin();
		int k = m - 1;
		while (mlist.size() > 1) {
			while (k--) {
				it++;
				if (it == mlist.end())
					it = mlist.begin();

			}
			it = mlist.erase(it);
			if (it == mlist.end())
				it = mlist.begin();
			k = m - 1;
		}
		return mlist.front();
	}
};
```



## 用两个栈实现队列(易)

**题目描述**

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

```c++
class Solution
{
public:
    void push(int node) {
        while(!stack1.empty()){
            stack2.push(stack1.top());
            stack1.pop();
        }
        stack1.push(node);
        while(!stack2.empty()){
            stack1.push(stack2.top());
            stack2.pop();
        }
    }

    int pop() {
        int top = stack1.top();
        stack1.pop();
        return top;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```

## 包含min函数的栈

**题目描述**

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

{%asset_img 06.png%}

```c++
class Solution {
public:
    void push(int value) {
        dataStack.push(value);
        if(minStack.empty()){
            minStack.push(value);
        }
        else{
            if(value < minStack.top())
                minStack.push(value);
            else
                minStack.push(minStack.top());
        }
    }
    void pop() {
        dataStack.pop();
        minStack.pop();
    }
    int top() {
        return dataStack.top();
    }
    int min() {
        return minStack.top();
    }
private:
    std::stack<int> dataStack;
    std::stack<int> minStack;
};
```

## 栈的压入、弹出序列

**题目描述**

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

**思路:用一个栈来模拟压入弹出操作。**

如果下一个弹出的数字刚好是栈顶数字, 那么直接弹出;如果下一个弹出的数字不在栈顶, 则把**压栈序列**中还没有入栈的数字压入辅助栈,直到把下一个需要弹出的数字压入栈顶为止;如果所有数字都压入栈后仍然没有找到下一个弹出的数字, 那么该序列不可能是一个弹出序列。

```c++
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        if(pushV.size()==0)
            return false;
        for(int i=0,j=0;i<pushV.size();i++){
            dataStack.push(pushV[i]);
            while(j<popV.size()&&dataStack.top() == popV[j]){
                dataStack.pop();
                j++;
            }
        }
        return dataStack.empty();  //若压栈完了，没能全部弹出，说明不是弹出序列，反之是
    }
private:
    std::stack<int> dataStack;
};
```

