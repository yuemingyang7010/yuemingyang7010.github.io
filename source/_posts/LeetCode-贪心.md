---
title: '贪心'
date: 2019-06-24 13:52:09
tags:
	- 贪心刷题
categories: 
	- 数据结构与算法
	- 算法刷题
---

## 总结目录

| 题目                   | 难度 | 收获与总结                                                   |
| ---------------------- | ---- | ------------------------------------------------------------ |
| 1.分饼干               | 易   | 首先**对需求数组g[]和饼干大小数组s[]排序**，然后循环遍历，知道其中某一个数组遍历完，循环结束。两个数组都从首元素开始遍历过程中，**每次cookie加1，若能够满足其中一个孩子则child加1**，遍历完成后，返回child值。 |
| 2.摇摆数列             | 中   | 画**数字升降规律图**和用**状态机**的方法，眼前一亮，方法值得学习借鉴。 |
| 3.移除K位数字          | 中   | 思路：从最高位向最低位遍历，**当对应的数字比下一位数字大，并且在没有剔除完K位数时，则应该剔除该位数**，这样才能保证最后的数最小，编程时可以借用**栈**来实现。此外还要考虑数的开头不能为0的情况，以及遍历完字符串后，仍然没有删除完K位数的情况。 |
| 4.用最少的弓箭击爆气球 | 中   | 将**所有气球区间的左端点从小到大排序**，首先以第一个气球所在区间作为射击区间，遍历第二个气球的时候**更新该区间**，依次不断更新，当遍历到第i个气球时，气球区间左端点大于射击区间右端点，此时设定第二个射击区间，以此类推。 |

## Assign Cookies(易)

455. Assign Cookies

**分发饼干**

{%asset_img 01.png%}

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor gi, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size sj. If sj >= gi, we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Note:**

You may assume the greed factor is always positive. 

You cannot assign more than one cookie to one child.

**Example 1:**

**Input:** [1,2,3], [1,1]  

**Output:** 1  

**Explanation:** You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3.  And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content. You need to output 1. 

**Example 2:**

**Input:** [1,2], [1,2,3]  

**Output:** 2  

**Explanation:** You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2.  You have 3 cookies and their sizes are big enough to gratify all of the children,  You need to output 2.

{%asset_img 02.png%}

{%asset_img 03.png%}

{%asset_img 04.png%}

{%asset_img 05.png%}

```c++
#include <vector>
#include <algorithm>
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        std::sort(g.begin(),g.end());
        std::sort(s.begin(),s.end());
        int child = 0;
        int cookie = 0;
        while(child < g.size() && cookie < s.size()){
            if(g[child]<=s[cookie]){
                child++;
            }
            cookie++;
        }
        return child;
    }
};
```

## Wiggle Subsequence(中)

摇摆序列

376. Wiggle Subsequence

{%asset_img 06.png%}

A sequence of numbers is called a **wiggle sequence** if the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with fewer than two elements is trivially a wiggle sequence.

For example, **[1,7,4,9,2,5]** is a wiggle sequence because the differences (6,-3,5,-7,3) are alternately positive and negative. In contrast, [1,4,7,2,5] and [1,7,4,5,5] are not wiggle sequences, the first because its first two differences are positive and the second because its last difference is zero.

Given a sequence of integers, return the length of the longest subsequence that is a wiggle sequence. A subsequence is obtained by deleting some number of elements (eventually, also zero) from the original sequence, leaving the remaining elements in their original order.

**Example 1:**

**Input:** [1,7,4,9,2,5] 

**Output:** 6

 **Explanation:** The entire sequence is a wiggle sequence.

**Example 2:**

**Input:** [1,17,5,10,13,15,10,5,16,8] 

**Output:** 7 

**Explanation:** There are several subsequences that achieve this length. One is [1,17,10,13,10,16,8].

**Example 3:**

**Input:** [1,2,3,4,5,6,7,8,9] 

**Output:** 2

**Follow up:**

Can you do it in O(*n*) time?

{%asset_img 07.png%}

{%asset_img 08.png%}

{%asset_img 09.png%}

```c++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size() < 2){
            return nums.size();
        }
        const int BEGIN = 0;
        const int UP = 1;
        const int DOWN = 2;
        int STATE = BEGIN;
        int maxLength = 1;
        for(int i=1;i<nums.size();i++){
            switch(STATE){
                case BEGIN:
                    if(nums[i]>nums[i-1]){
                        STATE = UP;
                        maxLength++;
                    }
                    else if(nums[i]<nums[i-1]){
                        STATE = DOWN;
                        maxLength++;
                    }
                    break;
                case UP:
                    if(nums[i]<nums[i-1]){
                        STATE = DOWN;
                        maxLength++;
                    }
                    break;
                case DOWN:
                    if(nums[i]>nums[i-1]){
                        STATE = UP;
                        maxLength++;
                    }
                    break;
            }
        }
        return maxLength;
    }
};
```

## Remove K Digits(中)

移掉K位数字

402. Remove K Digits

{%asset_img 10.png%}

Given a non-negative integer *num* represented as a string, remove *k* digits from the number so that the new number is the smallest possible.

**Note:**

- The length of *num* is less than 10002 and will be ≥ *k*.
- The given *num* does not contain any leading zero.

**Example 1:**

Input: num = "1432219", k = 3 

Output: "1219" 

Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest. 

**Example 2:**

Input: num = "10200", k = 1 

Output: "200" 

Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes. 

**Example 3:**

Input: num = "10", k = 2 

Output: "0" 

Explanation: Remove all the digits from the number and it is left with nothing which is 0.

{%asset_img 11.png%}

{%asset_img 12.png%}

{%asset_img 13.png%}

{%asset_img 14.png%}

{%asset_img 15.png%}

{%asset_img 16.png%}

自己实现的代码比上述截图稍微简洁点。

```c++
#include <stack>
class Solution {
public:
    string removeKdigits(string num, int k) {
        std::stack<char> mStack;
        string ans = "";
        for(int i=0;i<num.length();i++){    
            //当栈不空，且要压入的数字比栈顶数字小，且仍然可以删除数字的时候，while循环继续
            while((!mStack.empty()) && num[i] < mStack.top() &&  k>0){
                mStack.pop();
                k--;
            }
            if(num[i] != '0' || (!mStack.empty())){ //防止出现数字字符串以0开头
               mStack.push(num[i]); 
            }                     
        }
        //解决当字符串从头遍历到尾，k仍然大于0，如nums= "12345" k=3时，此时弹出末尾比较大的数
        while((!mStack.empty()) && k>0){ //如果栈不空，且仍然可以删除数字
            mStack.pop();
            k--;
        }
        while(!mStack.empty()){   //将栈中的每一个char型字符连接为字符串
            ans = mStack.top() + ans;
            mStack.pop();
        }
        if(ans == ""){   //根据题目要求，当ans为空字符串时候，返回“0”
            ans = "0";
        }
        return ans;
    }
};
LeetCode优秀解答，没有用到额外的数据结构，但是纯用数组比较绕！！！
class Solution {
public:
    string removeKdigits(string num, int k) {
        if (num.size()<=k)return "0";
        int top=0,count=0,n=num.size();
        for (int i=0;i<n;i++){
            while (top>0&&count<k&&num[top-1]>num[i]){
                top--;
                count++;
            }
            num[top++]=num[i];
        }
        top=min(top,n-k);
        int i=0;
        for (;i<top;i++){
            if (num[i]!='0')break;
        }
        if (i==top)return "0";
        string res="";
        for (;i<top;i++)
            res+=num[i];
        return res;
    }
};
```

## Minimum Number

452. Minimum Number of Arrows to Burst Balloons

{%asset_img 17.png%}

There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most 104 balloons.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.

**Example:**

**Input:** [[10,16], [2,8], [1,6], [7,12]]  

**Output:** 2  

**Explanation:** One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons). 

{%asset_img 18.png%}

{%asset_img 19.png%}

{%asset_img 20.png%}

{%asset_img 21.png%}

{%asset_img 22.png%}

{%asset_img 23.png%}

```c++
#include <algorithm>
#include <vector>
bool comp(const pair<int, int>& a,const pair<int, int>& b){
    return a.first < b.first;
}
class Solution {
public:
    int findMinArrowShots(vector<pair<int, int>>& points) {
        if(points.size() == 0){
            return 0;
        }
        std::sort(points.begin(),points.end(),comp);
        int arrowNum = 1;
        int begin = points[0].first;
        int end = points[0].second;
        for(int i=1;i<points.size();i++){
            if(points[i].first <= end){
                begin = points[i].first;
                if(points[i].second < end){
                    end = points[i].second;
                }
            }
            else{
                arrowNum++;
                begin = points[i].first;
                end = points[i].second;
            }
        }
        return arrowNum;
    }
};
```

