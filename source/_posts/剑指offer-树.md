---
title: '剑指offer-树'
date: 2019-06-19 13:52:09
tags:
	- 剑指offer
categories: 
	- 数据结构与算法
	- 剑指offer
---

## 目录

| 题目                                                         | 难度 |                                                              |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| [7.重建二叉树.note](note://8DDF3522701B494690B73B66DAD632FD) | ☆☆☆  | 采用递归的方式不断找出根节点和左右子树                       |
| [树的子结构.note](note://053AB99ED0AE4E2BB8B2998BD0231A5C)   | ☆☆☆☆ | 第一步在树A中找到和B的根结点的值一样的结点R,第二步再判断树A中以R为根节点的子树是不是包含和树B一样的结构。 |
| [二叉树的镜像.note](note://BB74C82F4C8142CC98BD69937C0185AD) | ☆☆   | 先交换根节点的左右子节点，再将子节点作为根节点进行递归镜像操作。 |
| [从上往下打印二叉树.note](note://604E14887ACB4A54B2DB840DF409A161) | ☆    | 根节点入队列，然后出队列，出队时将其左右孩子入队，循环操作进行队列出队，每次出队将其左右孩子入队。当队列为空时，整棵树层序遍历完毕。 |
| [二叉搜索树的后序遍历序列.note](note://3FE09CB0553C4995AD5C56B1C18156BD) | ☆☆   | **BST的后序序列的合法序列**是，对于一个序列S，最后一个元素是x （也就是根），如果去掉最后一个元素的序列为T，那么T满足：T可以分成两段，前一段（左子树）小于x，后一段（右子树）大于x，且这两段（子树）都是合法的后序序列。完美的递归定义。 |
| [36.二叉搜索树与双向链表.note](note://D0DBBA55ED06432BA749C0EAAF85AFBA) | ☆☆   | 改造中序遍历，设置一个pre和cur，将中序遍历打印的过程替换为前后连接pre和cur |
| [二叉树深度.note](note://1A9A17C46DAF4E429528B2BA288CEBC1)   | ☆    | 采用尾递归的方式                                             |
| [55.判断树是否为平衡二叉树.note](note://5138177E05AC400A82052CE734DB2196) | ☆☆   | 性质：是一 棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。  **因此递归函数每次要计算出子树的高度。** |
|                                                              |      |                                                              |

## 重建二叉树

**题目描述**

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

**解题思路：采用递归方法**

{%asset_img 01.png%}

```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        return reConstruct(pre,0,pre.size()-1,vin,0,vin.size()-1);
    }
    TreeNode* reConstruct(vector<int> pre,int startPre,int endPre,vector<int> vin,int startIn,int endIn){
        if(startPre > endPre||startIn > endIn){
            return NULL;
        }
        TreeNode* root = new TreeNode(pre[startPre]);
        for(int i=startIn;i<=endIn;i++){
            if(vin[i] == pre[startPre]){
                root->left = reConstruct(pre,startPre + 1,startPre+i-startIn,vin,startIn,i-1);
                root->right = reConstruct(pre,startPre+i-startIn+1,endPre,vin,i+1,endIn);
                break;
            }
        }
        return root;
    }
};
```

## 树的子结构

**题目描述**

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

**解题思路**

要查找树A中是否存在和树B结构一样的子枘我们可以分为两步：第一步在树A中找到和B的根结点的值一样的结点R,第二步再判断树A中以R为根节点的子树是不是包含和树B一样的结构。

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)  //二叉树的前序遍历
    {
        bool result = false;
        if(pRoot1 != NULL && pRoot2 != NULL){
            if(pRoot1->val == pRoot2->val)
                result = DoseTreeHaveTree2(pRoot1,pRoot2);
            if(!result)
                result = HasSubtree(pRoot1->left,pRoot2);
            if(!result)
                result = HasSubtree(pRoot1->right,pRoot2);
        }
        return result;
    }
    //判断以pRoot1为根节点的树中是否有以pRoot2为根节点的树
    bool DoseTreeHaveTree2(TreeNode* pRoot1, TreeNode* pRoot2){
        if(pRoot2 == NULL)
            return true;
        if(pRoot1 == NULL || pRoot1->val != pRoot2->val)
            return false;
        return DoseTreeHaveTree2(pRoot1->left,pRoot2->left)&&DoseTreeHaveTree2(pRoot1->right,pRoot2->right);
    }
};
```

## 二叉树的镜像

**题目描述**

操作给定的二叉树，将其变换为源二叉树的镜像。

**输入描述:**

二叉树的镜像定义：

源二叉树      	   

​         8     	   

​       /  \     	  

​    6   10     	 

   /  \  /  \     	

 5  7 9  11     	

镜像二叉树     	    

​         8     	  

​        /  \     	  

​     10   6     	 

​     /  \  /  \     	

  11  9  7   5

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    void Mirror(TreeNode *pRoot) {
        if(pRoot != NULL){
            TreeNode *temp = pRoot->left;
            pRoot->left = pRoot->right;
            pRoot->right = temp;
            if(pRoot->left){
                Mirror(pRoot->left);
            }
            if(pRoot->right){
                Mirror(pRoot->right);
            }
        }
    }
};
```

## 从上往下打印二叉树

**题目描述**

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

思路：参考[二叉树的递归与非递归遍历（前序、中序、后序、层序）](note://wcp153494909997195)

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> result;
        if(root == NULL)
            return result;
        mQueue.push(root);
        while(!mQueue.empty()){
            TreeNode* temp = mQueue.front();
            result.push_back(temp->val);
            mQueue.pop();
            if(temp->left)
                mQueue.push(temp->left);
            if(temp->right)
                mQueue.push(temp->right);
        }
        return result;
    }
private:
    queue<TreeNode*> mQueue;
};
```

## 二叉搜索树的后序遍历序列

**题目描述**

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

**思路：**

BST的后序序列的合法序列是，对于一个序列S，最后一个元素是x （也就是根），如果去掉最后一个元素的序列为T，那么T满足：T可以分成两段，前一段（左子树）小于x，后一段（右子树）大于x，且这两段（子树）都是合法的后序序列。完美的递归定义 : ) 。

```c++
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        if(sequence.size() == 0)
            return false;
        return juge(sequence,0,sequence.size()-1);
    }
    bool juge(vector<int> sequence,int left,int right){
        if(left >= right)
            return true;
        //从右向左寻找右子树所对应的序列
        int i=right-1;
        for(;i>=left && sequence[i]>sequence[right];i--);
        for(int j=i;j>=left;j--){  //寻找左子树对应的序列
            if(sequence[j]>sequence[right])
                return false;
        }
        //递归判断左子树和右子树是不是都符合条件，注意最后一个参数为right-1 ！！！
        return juge(sequence,left,i)&&juge(sequence,i+1,right-1);
    }
};
```

## 二叉搜索树与双向链表

**题目描述**

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

**解题思路：**

该结构特点可以类比到双向链表中： 

在双向链表中，每个结点也有两个指针，它们分别指向前一个结点和后一个结点。

所以这两种数据结构的结点是一致，二叉搜索树之所以为二叉搜索树，双向链表之所以为双向链表，只是因为两个指针的指向不同而已，通过改变其指针的指向来实现是完全可能的。

{%asset_img 02.png%}

**具体实现步骤：** 

原先指向左子结点的指针调整为链表中指向前一个结点的指针，原先指向右子结点的指针调整为链表中指向下一个结点的指针。 

具体转换过程：按照中序遍历的方法遍历二叉搜索树，可以将该二叉树分为三个部分：根节点、左子树和右子树，当遍历结点值为4的节点时，将它分为以2为节点的左子树和以6为节点的右子树，并将4的左指针指向值为3的结点，值为3的节点的右指针指向值为4的结点，因为采用的是中序遍历，所以当遍历到根节点的时候，它的左子树已经遍历结束了，所以要对所有的子树采用递归的执行上述操作

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        TreeNode* pre = NULL;
        Tree2list(pRootOfTree,&pre);  //运行完之后pre对应最后一个节点
        TreeNode* pHead = pre;
        while(pHead != NULL && pHead->left != NULL)
            pHead = pHead->left;
        return pHead;
    }
    
    void Tree2list(TreeNode* root,TreeNode** pre){
        if(root == NULL)
            return;
        TreeNode* cur = root;
        if(cur->left != NULL)
            Tree2list(cur->left,pre);
        
        cur->left = *pre;   //当前节点连接前一个节点
        if(*pre != NULL)    //前一个节点连接当前节点
            (*pre)->right = cur;
        *pre = cur;        //更新当前节点为前一个节点
        
        if(cur->right != NULL)
            Tree2list(cur->right,pre);
    }
};
```

## 二叉树深度

**题目描述**

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

**思路：采用递归的方式**

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    int TreeDepth(TreeNode* pRoot)
    {
        if(pRoot == NULL)
            return 0;
        int lDepth = TreeDepth(pRoot->left);
        int rDepth = TreeDepth(pRoot->right);
        return lDepth>rDepth?(lDepth+1):(rDepth+1);
    }
};
```

## 判断树是否为平衡二叉树

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

平衡二叉树性质：是一 棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

思路：

方法一（不合适，需要重复遍历节点多次，耗时长O(n^2)））

```c++
bool isBalanced(TreeNode* pRoot){
	if(pRoot == NULL)
		return true;
	int left = maxDepth(pRoot->left);
	int right = maxDepth(pRoot->right);
	int diff = left-right;
	if(diff <-1 || diff >1)
		return false;
	return isBalanced(pRoot->left)&&isBalanced(pRoot->right);
}
```

**方法二：**

用后序遍历的方式遍历整棵二叉树。在遍历某节点的左、右子节点之后，我们可以根据它的左、右子节点的深度判断它是不是平衡的，并得到当前节点的深度。当最后遍历到树的根节点的时候，也就判断了整棵二叉树是不是平衡二叉树。

 //**后续遍历**时，遍历到一个节点，其左右子树已经遍历  依次自底向上判断，每个节点只需要遍历一次

```c++
class Solution {
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        int depth = 0;
        return isBalanced(pRoot,depth);
    }
    bool isBalanced(TreeNode* pRoot,int &depth){
        if(pRoot == NULL)
            return true;
        int left = 0;
        int right = 0;
        if(isBalanced(pRoot->left,left) && isBalanced(pRoot->right,right)){
            int dif = left-right;
            if(dif < -1 || dif > 1)
                return false;
            depth = left > right?(left+1):(right+1);
            return true;
        }
        return false;
    }
};
```

