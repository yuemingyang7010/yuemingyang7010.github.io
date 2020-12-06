---
title: LeetCode刷题
date: 2019-07-30 23:21:56
tags:
	- LeetCode刷题
categories: 
	- 数据结构与算法
	- 算法刷题
---

### 

### 两数之和(1.易)

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

两遍遍历：

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> m;
        vector<int> res;
        for (int i = 0; i < nums.size(); ++i) {
            m[nums[i]] = i; //先遍历一遍数组，建立HashMap映射
        }
           //然后再遍历一遍，开始查找，找到则记录index
        for (int i = 0; i < nums.size(); ++i) {
            int t = target - nums[i];
            if (m.count(t) && m[t] != i) {//if里面的条件用于判断查找到的数字不是第一个数字
                res.push_back(i);
                res.push_back(m[t]);
                break;
            }
        }
        return res;
    }
};
```

一次遍历：

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> m;
        for (int i = 0; i < nums.size(); ++i) {
            if (m.count(target - nums[i])) {
                return {i, m[target - nums[i]]};
            }
            m[nums[i]] = i;
        }
        return {};
    }
};
```

### 最小因式分解(625. 中)

给定一个正整数 a，找出最小的正整数 b 使得 b 的所有数位相乘恰好等于 a。如果不存在这样的结果或者结果不是 32 位有符号整数，返回 0。

样例 1

输入：48 
输出：68


样例 2

输入：15
输出：35

思路：

当该数小于10，直接返回该数，其他数分解出的因数一定是个位数字，即范围是[2, 9]。那我们就可以从大到小开始找因数，首先查找9是否是因数，是要能整除a，就是其因数，如果是的话，就加入到结果res的末尾，a自除以9，我们用while循环查找9，直到取出所有的9，然后取8，7，6...以此类推，如果a能成功的被分解的话，最后a的值应该为1，如果a值大于1，说明无法被分解，返回0。最后还要看我们结果res字符转为整型是否越界，越界的话还是返回0，参见代码如下：

```c++
class Solution {
public:
    int smallestFactorization(int a) {
        if (a < 10) return a;
        long long res = 0, cnt = 1;
        for (int i = 9; i >= 2; --i) {
            while (a % i == 0) {
                res += cnt * i;
                if (res > INT_MAX) return 0;
                a /= i;
                cnt *= 10;
            }
        }
        return (a == 1) ? res : 0;
    }
};
```

### 无重复字符的最长子串(3.中)

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

思路：滑动窗口+HashMap

​		例如"abcabcbb"，让你手动找无重复字符的子串，比如子串a，b，c，然后又出现了一个a，那么此时就应该去掉第一次出现的a，然后继续往后，又出现了一个b，则应该去掉一次出现的b，以此类推，最终发现最长的长度为3。

​		用HashMap来建立字符和其出现位置之间的映射。进一步考虑，由于字符会重复出现，到底是保存所有出现的位置呢，还是只记录一个位置？我们之前手动推导的方法实际上是维护了一个**滑动窗口**，**窗口内的都是没有重复的字符**，我们需要尽可能的扩大窗口的大小。由于窗口在不停向右滑动，所以**我们只关心每个字符最后出现的位置，并建立映射。窗口的右边界就是当前遍历到的字符的位置**，为了求出窗口的大小，我们需要一个**变量left来指向滑动窗口的左边界**，这样，如果当前遍历到的字符从未出现过，那么直接扩大右边界，**如果之前出现过**，那么就**分两种情况，在或不在滑动窗口内**，如果不在滑动窗口内，那么就没事，当前字符可以加进来，如果在的话，就需要先在滑动窗口内去掉这个已经出现过的字符了，去掉的方法并不需要将左边界left一位一位向右遍历查找，由于我们的HashMap已经保存了该重复字符最后出现的位置，所以**直接移动left指针就可以**了。我们维护一个结果res，每次用出现过的窗口大小来更新结果res，就可以得到最终结果啦。

​		这里解释下程序中那个if条件语句中的两个条件m.count(s[i]) && m[s[i]] > left，因为一旦当前字符s[i]在HashMap已经存在映射，说明当前的字符已经出现过了，而若m[s[i]] > left 成立，说明之前出现过的字符在我们的窗口内，那么如果要加上当前这个重复的字符，就要移除之前的那个，所以我们**让left赋值为m[s[i]]**，由于left是窗口左边界的前一个位置（这也是left初始化为-1的原因，因为窗口左边界是从0开始遍历的），所以相当于已经移除出滑动窗口了。举一个最简单的例子"aa"，当i=0时，我们建立了a->0的映射，并且此时结果res更新为1，那么当i=1的时候，我们发现a在HashMap中，并且映射值0大于left的-1，所以此时left更新为0，映射对更新为a->1，那么此时i-left还为1，不用更新结果res，那么最终结果res还为1，正确，代码如下：

解法一：

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(s.length()==0)
            return 0;
        int res=0,left=-1,len=s.length(); //left代表滑窗左边界元素的前一个元素位置
        unordered_map<int,int> m;  //key为字符串的每一个字符，value为该字符从左到右遍历过程中最后出现的位置
        for(int i=0;i<len;i++){
            if(m.count(s[i]) && m[s[i]]>left)  //如果满足字符在m中，并且还在滑动窗口中
                left = m[s[i]];      //更新左边界的前一个元素位置
            m[s[i]] = i;   //更新s[i]对应的位置，及s[i]最后出现的位置
            res = max(res,i-left);
        }
        return res;
    }
};
```

解法二：（本质和解法一完全一样）

解法二是解法一的精简模式，这里我们可以建立一个256位大小的整型数组来代替HashMap，然后我们全部初始化为-1，这样的好处是我们就不用像之前的HashMap一样要查找当前字符是否存在映射对了，对于每一个遍历到的字符，我们直接用其在数组中的值来更新left，因为默认是-1，而left初始化也是-1，所以并不会产生错误，这样就省了if判断的步骤，其余思路都一样：

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int m[256]={-1};
        int res = 0, left = -1;
        for (int i = 0; i < s.size(); ++i) {
            left = max(left, m[s[i]]);  //因为数组m初始值为-1，因此只有
//满足其值不为-1（即该字符出现过），并且m[s[i]]大于left的时候才会left = m[s[i]]; 
            m[s[i]] = i;
            res = max(res, i - left);
        }
        return res;
    }
};
```

### 两数相加(2.中)

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

思路：

new一个头结点，方便以后的操作

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
        ListNode* preHead = new ListNode(-1);    //头结点,方便后续的操作
        int carry = 0;
        ListNode *cur = preHead;
        while(l1||l2){
            int ai = l1 == nullptr ? 0 : l1->val;  //l1对应位置的值，如果不存在，则补0
            int bi = l2 == nullptr ? 0 : l2->val;  //同上
            int value = (ai + bi + carry) % 10;    //求相应加法运算后值
            carry = (ai + bi + carry) / 10;        //求相应加法运算后进位值
            cur->next = new ListNode(value);      //尾插法   
            
            //更新
            l1 = l1 == nullptr ? nullptr : l1->next;  
            l2 = l2 == nullptr ? nullptr : l2->next;
            cur = cur->next;         
            
        }
        if (carry > 0)     //判断最后是否有进位，如果有，多开一个节点
            cur->next = new ListNode(carry);
        return preHead->next;
    }
};
```

### 最长回文子串(5.中)

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

### 反转链表(206. 易)

反转一个单链表。

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

思路：

{%asset_img 01.png%}

迭代法：

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
        ListNode *newHead = NULL;
        while (head) {
            ListNode *t = head->next;
            head->next = newHead;
            newHead = head;
            head = t;
        }
        return newHead;
    }
};
```

递归法：

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
        ListNode* p = reverseList(head->next); //把head节点之后的所有节点都反转了
        head->next->next = head; //head->next为原先head节点后面部分的首节点，反转后变成尾节点，所以他的下一个节点接上head节点，至此整个链表反转完成
        head->next = NULL;   //此时head为新链表尾节点，其下一个节点需要置为NULL
        return p;
    }
};
```

### 最大子数组和(53. 易)

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: **连续子数组** [4,-1,2,1] 的和最大，为 6。
进阶:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

思路：

动态规划：

dp[i] = max(dp[i-1]+nums[i],nums[i]);  //dp[i]代表以位置i元素为结尾的子数组的最大和

然后求出最大的dp[i]

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        int maxAns;
        vector<int> dp(n,0);   //dp[i]代表以位置i元素为结尾的子数组的最大和
        if(n == 0){
            return 0;
        }
        else if(n == 1){
            return nums[0];
        }
        dp[0] = nums[0];
        maxAns = nums[0];
        for(int i=1;i<n;i++){
            dp[i] = max(dp[i-1]+nums[i],nums[i]);
            if(dp[i] > maxAns){
                maxAns = dp[i];
            }
        }
        return maxAns;
    }
};
```

### 三数之和（15.中）

给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]

思路：

​		我们对原数组进行**排序**，然后开始遍历排序后的数组，这里注意不是遍历到最后一个停止，而是到倒数第三个就可以了。这里我们可以先做个剪枝优化，就是**当遍历到正数的时候就break**，为啥呢，因为我们的数组现在是有序的了，如果第一个要fix的数就是正数了，那么后面的数字就都是正数，就永远不会出现和为0的情况了。然后我们还要加上**重复就跳过的处理**，处理方法是从第二个数开始，如果和前面的数字相等，就跳过，因为我们不想把相同的数字fix两次。

​		对于遍历到的数，用0减去这个fix的数得到一个target，然后只需要再之后找到两个数之和等于target即可。我们用两个指针分别指向fix数字之后开始的数组首尾两个数，如果两个数和正好为target，则将这两个数和fix的数一起存入结果中。然后就是跳过重复数字的步骤了，两个指针都需要检测重复数字。如果两数之和小于target，则我们将左边那个指针i右移一位，使得指向的数字增大一些。同理，如果两数之和大于target，则我们将右边那个指针j左移一位，使得指向的数字减小一些，代码如下：

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int> > res;
        sort(nums.begin(),nums.end());  //先排序
        if(nums.empty()||nums.back()<0||nums.front()>0)
            return {};  //数组为空、全为正数或者全为负数，结果都返回空
        for(int k=0;k<nums.size();++k){
            if(nums[k]>0)    //当用来计算target的元素大于0的时候，证明后面的两正数之和小于0，不可能的情况
                break;
            if(k>0 && nums[k]==nums[k-1])  //遇到用来计算target的元素相等的情况，去掉重复
                continue;
            int target = 0-nums[k];
//下面一部分其实成了用左右双指针求和为target的两个数。
            int i=k+1;
            int j=nums.size()-1;
            while(i<j){
                if(nums[i]+nums[j]==target){
                    res.push_back({nums[k],nums[i],nums[j]});
                    while(i<j&&nums[i]==nums[i+1])  //去处重复情况
                        i++;
                    while(i<j&&nums[j]==nums[j-1])  //去处重复情况
                        j--;
                    i++;
                    j--;
                }
                else if(nums[i] + nums[j] < target)
                    i++;
                else 
                    --j;
            }
        }
        return res;
    }
};
```

### 有效的括号(20. 易)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

输入: "()"
输出: true
示例 2:

输入: "()[]{}"
输出: true
示例 3:

输入: "(]"
输出: false
示例 4:

输入: "([)]"
输出: false
示例 5:

输入: "{[]}"
输出: true



思路：

这道题让我们验证输入的字符串是否为括号字符串，包括大括号，中括号和小括号。这里我们需要**用一个栈**，我们开始遍历输入字符串，如果**当前字符为左半边括号时，则将其压入栈中**，如果**遇到右半边括号时，若此时栈为空，则直接返回false，如不为空，则取出栈顶元素**，若为对应的左半边括号，则继续循环，反之返回false，代码如下：

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> mstack;
        for(int i=0;i<s.length();i++){
            if(s[i] == '('||s[i] == '['||s[i] == '{')
                mstack.push(s[i]);
            else{
                if(mstack.empty()) return false;
                if(s[i] == ')'&&(mstack.top() != '(')) return false;
                if(s[i] == ']'&&(mstack.top() != '[')) return false;
                if(s[i] == '}'&&(mstack.top() != '{')) return false;
                mstack.pop();
            }
        }
        return mstack.empty();
    }
};
```

