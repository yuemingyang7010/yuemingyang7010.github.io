---
title: '链表'
date: 2019-06-22 13:52:09
tags:
	- 链表刷题
categories: 
	- 数据结构与算法
	- 算法刷题
---

## 总结目录

| 题目                   | 难度 | 收获与分析                                                   |
| ---------------------- | ---- | ------------------------------------------------------------ |
| 1.两数相加(链表表示的) | 中   |                                                              |
| 2.链表逆序             | 易   | 采用头插法，通常申请一个ListNode*新的头指针。                |
| 3.链表逆序II           | 中   | 类似2，只是需要头尾的连接，保存好需要逆序的序列前驱元素指针和后面元素的首节点指针。 |
| 4.相交链表             | 易   | 方法1：用set,简单，但是需要开辟额外的空间，空间复杂度O(n);方法2：先统计两个链表的长度，然后再遍历。 |
| 5.判断链表是否存在环   | 易   | 方法1：用set,很好，但是用了额外空间；方法2：用快慢指针法。   |
| 6.返回链表环的起始节点 | 中   | 类似5，也有两种方法，重点记住，相遇节点和头结点同样速度朝前遍历，相遇的地方，即为环的起始节点。 |
| 7.划分链表             | 中   | 采用尾插法，额外申请两个ListNode*的节点，以及用于遍历的ListNode*指针，将节点val小于X的放在一个链表中，另一部分放... |
| 8.合并两个有序链表     | 易   | 同7，采用尾插法，额外申请1个ListNode*的节点，以及用于遍历的ListNode*指针... |

## Add Two Numbers(中)

You are given two **non-empty** linked lists representing two non-negative integers. The digits（数字） are stored（存储） in **reverse order** （逆序）and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```c++
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

**代码：**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode dummy(-1);   //头结点，（自己代码缺少头结点，导致后面不好操作）
        int carry = 0;
        ListNode *prev = &dummy;
        for (ListNode *pa = l1, *pb = l2;pa != nullptr || pb != nullptr;
        pa = pa == nullptr ? nullptr : pa->next,
        pb = pb == nullptr ? nullptr : pb->next,
        prev = prev->next) {
            const int ai = pa == nullptr ? 0 : pa->val;
            const int bi = pb == nullptr ? 0 : pb->val;
            const int value = (ai + bi + carry) % 10;
            carry = (ai + bi + carry) / 10;
            prev->next = new ListNode(value);    //尾插法
        }
        if (carry > 0)     //判断最后是否有进位，如果有，多开一个节点
            prev->next = new ListNode(carry);
        return dummy.next;
    }
};
```

**复杂度分析**

- 时间复杂度：*O*(max(*m*,*n*))，假设 *m* 和 *n* 分别表示 *l*1 和 *l*2 的长度，上面的算法最多重复 max(*m*,*n*)次。
- 空间复杂度：*O*(max(*m*,*n*))， 新列表的长度最多为 max(*m*,*n*)+1。

## Reverse Linked List（易）

反转一个单链表。

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

思路分析：

{%asset_img 01.png%}

{%asset_img 02.png%}

{%asset_img 03.png%}

{%asset_img 04.png%}

**迭代版本：**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = NULL; //反转后的链表头，相对于旧的链表头为pre  
        ListNode* cur = head; //旧的链表头，为当前节点
        while(cur){
            ListNode* tempNext = cur->next; //临时存储旧链表头的下一个节点
            cur->next = pre; //旧链表头指向新链表头
            pre = cur;  //新链表头指针pre朝前移动一位
            cur = tempNext; //旧链表头更新为旧链表头的下一位                 
        }
        return pre;
    }
};
```

**递归版本：**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == NULL || head->next == NULL)
            return head;
        ListNode* p = reverseList(head->next); //从第二个节点开始反转
        head->next->next = head;      //上一步第二个节点反转完了应该处在链表尾，这个时候他的下一个节点接上head节点
        head->next = NULL;   //head->next信息清掉
        return p;
    }
};
```

2.在剑指offer上给出的是在不改变链表的结构情况下，逆序打印链表，分用栈和递归两种方式实现

(1)用栈来实现链表的逆序打印

(2)用递归实现

```c++
// 面试题6：从尾到头打印链表
// 题目：输入一个链表的头结点，从尾到头反过来打印出每个结点的值。

#include "..\Utilities\List.h"
#include <stack>

void PrintListReversingly_Iteratively(ListNode* pHead)
{
    std::stack<ListNode*> nodes;

    ListNode* pNode = pHead;
    while(pNode != nullptr)
    {
        nodes.push(pNode);
        pNode = pNode->m_pNext;
    }
    while(!nodes.empty())
    {
        pNode = nodes.top();
        printf("%d\t", pNode->m_nValue);
        nodes.pop();
    }
}

void PrintListReversingly_Recursively(ListNode* pHead)
{
    if(pHead != nullptr)
    {
        if (pHead->m_pNext != nullptr)
        {
            PrintListReversingly_Recursively(pHead->m_pNext);
        }
        printf("%d\t", pHead->m_nValue);
    }
}
```

## Reverse Linked List II（中）

**链表逆序II**

\92. Reverse Linked List II

Reverse a linked list from position *m* to *n*. Do it in one-pass.

**Note:** 1 ≤ *m* ≤ *n* ≤ length of list.

**Example:**

**Input:** 1->2->3->4->5->NULL, *m* = 2, *n* = 4 

**Output:** 1->4->3->2->5->NULL

{%asset_img 05.png%}

{%asset_img 06.png%}

{%asset_img 07.png%}

{%asset_img 08.png%}

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        int reverseLength = n - m + 1;
        ListNode* pre_head = NULL;
        ListNode* modify_list_tail = NULL;
        ListNode* new_head = NULL;
        ListNode* ans_head = head;       
        while(head && --m){
            pre_head = head;
            head = head->next;
        }
        modify_list_tail = head;
        while(head && reverseLength--){
            ListNode* next = head->next;
            head->next = new_head;
            new_head = head;
            head = next;            
        }
        modify_list_tail->next = head;
        if(pre_head){    //pre_head为真代表m>1
            pre_head->next = new_head;
        }
        else{
            ans_head = new_head;
        }
        return ans_head;
    }
};
```

## Intersection(交点) of Two Linked

[**相交链表**](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

160. Intersection(交点) of Two Linked Lists

{%asset_img 16.png%}

Write a program to find the node at which the intersection of two singly linked lists begins.



For example, the following two linked lists:

```c++
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```

begin to intersect at node c1.



**Notes:**

- If the two linked lists have no intersection at all, return null.
- The linked lists must retain(保持) their original structure after the function returns.
- You may assume（假定） there are no cycles anywhere in the entire linked structure.
- Your code should preferably run in O(n) time and use only O(1) memory.

{%asset_img 09.png%}

{%asset_img 10.png%}

{%asset_img 11.png%}

{%asset_img 12.png%}

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
//用来计算链表的长度
int listLength(ListNode *head){
    int len = 0;
    while(head){
        head = head->next;
        len++;
    }
    return len;
}
//用来返回两个已知长度的链表的相交点的指针
ListNode *findIntersection(ListNode *headL, ListNode *headS,int lengthL,int lengthS){
    int lengthDelt = 0;
    lengthDelt = lengthL-lengthS;
    while(headL && lengthDelt--){
        headL = headL->next;
    }
    while(headS){
        if(headL == headS){
            return headL;
        }
        headL = headL->next;
        headS = headS->next;
    }
    return NULL;
}
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lengthA,lengthB;
        lengthA = listLength(headA);
        lengthB = listLength(headB);
        ListNode *ans;
        if(lengthA > lengthB){
            ans = findIntersection(headA, headB,lengthA,lengthB);
        }
        else{
            ans = findIntersection(headB, headA,lengthB,lengthA);
        }
        return ans;
    }
};
```

{%asset_img 13.png%}

{%asset_img 14.png%}

{%asset_img 15.png%}

## Linked List Cycle(易)

\141. Linked List Cycle

Given a linked list, determine if it has a cycle in it.

Follow up:

**Can you solve it without using extra space?**

**自己做法，用STL中的set,用了额外的空间。**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        std::set<ListNode*>node_set;
        int num = 0;
        while(head){
            num++;
            node_set.insert(head);
            if(node_set.size() != num){
                return true;
            }
            head = head->next;          
        }
        return false;
    }
};
```

快慢指针的实现方法如下（思想讲解可参考6中）：

```c++
  Node *slow = head, *fast = head;
  while (slow && fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
    if (slow == fast)
      return true;
  }
  return false;
}
```

## Linked List Cycle II（中）

{%asset_img 17.png%}

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

**Note:** Do not modify the linked list.

**Follow up**:

**Can you solve it without using extra space?**

**自己做法，用STL中的set,用了额外的空间，和5几乎一样。**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        std::set<ListNode*>node_set;
        int num = 0;
        while(head){
            num++;
            node_set.insert(head);
            if(node_set.size() != num){
                return head;
            }
            head = head->next;          
        }
        return NULL;
    }
};
```

**经典方法：快慢指针法**

{%asset_img 18.png%}

{%asset_img 19.png%}

{%asset_img 20.png%}

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* low = head;
        ListNode* meet = NULL;
        //先求出相遇点的位置
        while(fast && low && fast->next){
            fast = fast->next->next;
            low = low->next;
            if(fast == low){
                meet = fast;
                break;
            }
        }
        //再求出环的交点的位置
        while(head && meet){
            if(head == meet){
                return head;
            }
            head = head->next;
            meet = meet->next;
        }
        return NULL;       
    }
};
```

## Partition （划分）List(中)

{%asset_img 21.png%}

Given a linked list and a value *x*, partition it such that all nodes less than *x* come before nodes greater than or equal to *x*.

You should preserve the original relative order of the nodes in each of the two partitions.

**Example:**

**Input:** head = 1->4->3->2->5->2, *x* = 3 

**Output:** 1->2->2->4->3->5

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode  lessHead(0);       //设置两个临时节点
        ListNode  moreHead(0);
        ListNode* pLess = &lessHead; //设置两个指针用于对两个链表的尾插
        ListNode* pMore = &moreHead;
        while(head){
            if(head->val < x){
                pLess->next = head;
                pLess = pLess->next;
            }
            else{
                pMore->next = head;
                pMore = pMore->next;
            }
            head = head->next;
        }
        pLess->next = moreHead.next;
        pMore->next = NULL;         //此句要加，不然跑不过，纳闷中...
        return lessHead.next; 
    }
};
```

{%asset_img 22.png%}

{%asset_img 23.png%}

{%asset_img 24.png%}

{%asset_img 25.png%}

## Merge(合并) Two Sorted Lists(易)

{%asset_img 26.png%}

{%asset_img 27.png%}

{%asset_img 28.png%}

21. Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

Example:

Input: 1->2->4, 1->3->4

Output: 1->1->2->3->4->4

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* newHead = new ListNode(0);
        ListNode* pTemp  = newHead;
        while(l1 && l2){
            if(l1->val < l2->val){
                pTemp->next = l1;
                l1 = l1->next;
            }
            else{
                pTemp->next = l2;
                l2 = l2->next;
            }
            pTemp = pTemp->next;
        }
        //将比较后某一个原始链表剩余部分直接插到新的链表后方
        if(l1){   //如果l1有剩余
            pTemp->next  = l1;
        }
        if(l2){
            pTemp->next  = l2;
        }
        return newHead->next;
    }
};
```

