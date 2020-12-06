---
title: '剑指offer-数组'
date: 2019-06-18 13:52:09
tags:
	- 剑指offer
categories: 
	- 数据结构与算法
	- 剑指offer
---

### 天平与假币

有12枚硬币，其中有且只有1枚是假币，其重量与真币不同，但不知是重还是轻。现给定一祭没有砝码的天平，问至少需要多少次称量才能确保找到这枚假币？

**解释：**

随机将12枚硬币等分成3份，每份4枚；标记为A、B、C三份。
将A放于左侧，B放于右侧，用天平称量A和B，分三种情况：
1.天平平衡
2.A（左）比B（右）重

3.A（左）比B（右）轻口与2对称，只分析2即可

**核心讲解：**

{%asset_img 08.png%}

{%asset_img 07.png%}

**理论下界**
一次天平称量能得到左倾、右倾、平衡3种情况，则把一次称量当成一位编码，该编码是3进制的。问题转换为：需要多少位编码，能够表示12呢？
(1)由于12的轻重未知，有两种可能，因此，需要用3进制表示24。
	答：假定需要n位，则：3^n >= 24
(2)取对数后计算得到n = 2.89，这表示至少3次才能找到该假币。

## 目录

| 题目                                                         | 难度 |                                                              |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| [1.二维数组中的查找(..).note](note://93AEF8BA6AA7487889D90A38FEFB6EFB) | ☆    | 从右上角到左下角进行查找                                     |
| [11.旋转数组的最小数字.note](note://9934046F6A364AF2AB3F9E2001172611) | ☆☆☆  | 采用分治的思想，递归地将数组进行二分区，然后找到最小的数（时间复杂度logn   顺序查找O(n)） |
| [10.斐波那契数列.note](note://1D28A716E2644F2896A9783B4422806D) | ☆    | 很简单，考虑节省空间复杂度，用循环代替递归。                 |
| [21.调整数组顺序使奇数位于偶数前面.note](note://74A9846762214F99B65C28886B2F5886) | ☆    | 用两个指针，一个遍历数组，一个指向存放奇数的位置，都从0开始，偶数暂时放在另一个vector中，先将奇数存放在奇数指针指向的位置，最后再存偶数。 |
| [顺时针打印矩阵.note](note://04273EDFA38342CF8F045D9929092182) | ☆☆   | 考虑全面：m×n,m×1,1×n,1×1几种情况都要考虑；流程控制采用top、down、left、right四个变量来控制。 |
| [39.数组中出现次数超过一半的数字.note](note://54E9B5A378AF4CAA89BAAA4B51CC98F6) | ☆☆   | 在遍历数组时保存两个值：一是数组中一个数字，一是次数。遍历下一个数字时，若它与之前保存的数字相同，则次数加1，否则次数减1；若次数为0，则保存下一个数字，并将次数置为1。遍历结束后，所保存的数字即为所求。 |
| [40.最小的K个数.note](note://40706EC19933414FAD962C18B084C1EA) | ☆    | TOPk问题                                                     |
| [52.在排序数组中查找数字.note](note://AB0F15EF819F450B8CE63334ADC3170F) | ☆☆   | 考虑查找效率，总体思路是找到第一个K位置和最后一个K的位置，作差加一求出个数。可以采取递归方式二分查找，也可直接通过循环二分查找，不过大神的**通过查找K-0.5和K+0.5更加简洁高效。** |
| [56.数组中只出现一次的两个数字.note](note://533C7FFFB40C4CB49945680548E9F990) | ☆☆   | 先将所有数依次异或，结果和两个单一数异或相同，从右开始找到结果中第一个为1的位，以此为标准将数分成两类，再将两类分别异或，得到的就是两个单数。**注意大坑！！！ a & flag != 0;和(a & flag) !=0;结果不同。** |
| [57(2).和为S的连续正数序列.note](note://3D914AA5A5E54B25B59FD5140A85343E) | ☆☆   | 用两个数small和big分别表示序列的最小值和最大值。首先把small初始化为1，big初始化为2。如果从small到big的序列的和大于s，则可以从序列中去掉较小的值，也就是增大small的值。如果从small到big的序列的和小于s，则可以增大big，让这个序列包含更多的数字。因为这个序列至少要有两个数字，我们一直增加small到（1+s）/2为止。 |
| [57(1).和为S的两个数字.note](note://2266DA7FAFA247DC85E399041FA1EEDB) | ☆☆   | 类似与[57(2).和为S的连续正数序列.note](note://3D914AA5A5E54B25B59FD5140A85343E)做法，只不过，只不过本题左指针开始指向数组最左端，右指针指向最右端。 |
|                                                              |      |                                                              |
| [扑克牌顺子.note](note://1222CD0B205943C898C7F5CE8FD72F1B)   | ☆☆   | 第一步：排序；第二步：统计0的个数，统计间隙的个数；第三步:间隙数大于0个数返回false，间隙数小于0的个数，更新0的个数，继续统计后面的数字是否有间隙。同时在此过程中也要统计前后两个数是否有重复情况，重复返回false。 |
| [2.数组中重复的数字.note](note://8912A6F7345E4239A3BACA1DBBB3E926) | ☆☆   | 从头到尾依次扫描数组元素，当扫描到第i个元素m时，当m等于i时，继续遍历下一元素，当不等于i时，则拿他与第m个数n比较，如果m=n，则找到重复元素，返回true,否则交换两元素... |
| [3.构建乘积数组.note](note://637AF30B91D443ADB65E4DE36FD82A10) | ☆☆   | 求B[i]的时候分为两步，先求左侧所有的A相乘，再求右侧的所有的A相乘，最终将两者相乘即得相应的B。具体通过两次循环，累乘得到不同的B[i] |
|                                                              |      |                                                              |

## 二维数组中的查找

**题目描述**

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

**解题思路：**

首先选取数组中**右上角的数字**，

【1】如果该数字等于要查的数字，则查找结束；

【2】如果该数字大于要查找的数字，则剔除该数字所在的列,如(a)；

【3】如果该数字小于要查找的数字，则剔除该数字所在的行，如(c)

{%asset_img 01.png%}

代码实现如下：

```c++
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        int row = array.size();
        int col = array[0].size();
        int i = 0;
        int j = col-1;
        while(i < row && j >= 0){
            if(target < array[i][j]){
                j--;
            }
            else if(target > array[i][j]){
                i++;
            }
            else{
                return true;
            }
        }
        return false;
    }
};
```

## 旋转数组的最小数字

**题目描述**

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

**思路：**

正常情况：

{%asset_img 02.png%}

特殊情况：

（1）数组为排序好的数组，发现arry[left] < arry[right]则认为是排序好的数组，去首元素返回；

（2）arry[left] = arry[right] = arry[mid]时，用顺序遍历查找，如下

{%asset_img 03.png%}

```c++
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        int n = rotateArray.size();
        if(n == 0){
            return 0;
        }
        else if(n == 1){
            return rotateArray[0];
        }
        int left = 0;
        int right = n-1;
        int mid = 0; //用于特殊情况1：将排序好的数组前面0个元素旋转到后面
        while(rotateArray[left] >= rotateArray[right]){
            if(right - left ==1){
                mid = right;
                break;
            }
            mid = (left + right)/2;
            //特殊情况2：首尾中三个元素大小相等，只能采用顺序查找，如｛1，0，1，1，1｝ 和 ｛1，1， 1，0，1｝
            if(rotateArray[left] == rotateArray[right] && rotateArray[left] == rotateArray[mid])
                return minInorder(rotateArray,left,right);
            if(rotateArray[mid] >= rotateArray[left])
                left = mid;
            else if(rotateArray[mid] <= rotateArray[right])
                right = mid;
        }
        return rotateArray[mid];
    }
    int minInorder(vector<int> rotateArray,int left,int right){
        int result = rotateArray[left];
        for(int i=left + 1;i<=right;i++){
            if(result > rotateArray[i])
                result = rotateArray[i];
        }
        return result;
    }
};
```

## 斐波那契数列

**题目描述**

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39

**思路：**

用非递归的方式解题

```c++
class Solution {
public:
    int Fibonacci(int n) {
        int last2 = 0;
        int last1 = 1;
        int cur;
        if(n == 0)
            return 0;
        else if(n == 1)
            return 1;
        else{
            for(int i=n;i>=2;i--){
                cur = last1 + last2;
                last2 = last1;
                last1 = cur;
            }
            return cur;
        }        
    }
};
```

## 调整数组顺序使奇数位于偶数前面

**题目描述**

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的**相对位置不变。（书上的方法是顺序可变）**

**思路：**

顺序遍历vector,将偶数放在一个queue里面，遇到奇数按顺序覆盖之前的vector，待vector遍历完了，将queue中的数再放到vector的后面。

```c++
#include <queue>
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        queue<int> evenQueue;
        int n = array.size();
        if(n == 0||n==1)
            return;
        int oddIndex = 0;
        for(int i=0;i<n;i++){
            if(isEven(array[i])){
                evenQueue.push(array[i]);
            }
            else{
                array[oddIndex++] = array[i];
            }
        }
        while(oddIndex < n){
            array[oddIndex++] = evenQueue.front();
            evenQueue.pop();
        }
    }
    bool isEven(int number){
        return (number & 1) == 0;
    }
};
```

## 顺时针打印矩阵

**题目描述**

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

{%asset_img 04.png%}

```c++
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> result;
        int row = matrix.size();
        if(row == 0)
            return result;
        int col = matrix[0].size();
        int top = 0;
        int down = row-1;
        int left = 0;
        int right = col-1;
        while(top <= down && left <= right){
            for(int i=left;i<=right;i++)
                result.push_back(matrix[top][i]);
            top++;
            if(top > down || left > right) break;
            for(int i=top;i<=down;i++)
                result.push_back(matrix[i][right]);
            right--;
            if(top > down || left > right) break;
            for(int i=right;i>=left;i--)
                result.push_back(matrix[down][i]);
            down--;
            if(top > down || left > right) break;
            for(int i=down;i>=top;i--)
                result.push_back(matrix[i][left]);
            left++;
            if(top > down || left > right) break;
        }
        return result;
    }
};
```

## 数组中出现次数超过一半的数字

**题目描述**

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

分析：

如果将这个数组排序，那么中位数一定是要求得这数；

**思路：**

方法一：用sort先排序，然后中间数即为所求；时间复杂度O(nlogn)

方法二：采用用户“分形叶”思路（注意到目标数 超过数组长度的一半，对数组同时去掉两个不同的数字，到最后剩下的一个数就是该数字。如果剩下两个，那么这两个也是一样的，就是结果），在其基础上把最后剩下的一个数字或者两个回到原来数组中，将数组遍历一遍统计一下数字出现次数进行最终判断。

方法三：借鉴快排思想，用时间复杂度为O(n)的partition()函数实现，但是总体的复杂度可能最差时候达到O(n^2) （剑指offer的方法）

```c++
class Solution {
public:
	int MoreThanHalfNum_Solution(vector<int> numbers) {
		if (numbers.size() <= 0)
			return 0;
		int midIndex = numbers.size() >>1;
		int index = 0;
		int start = 0;
		int end = numbers.size() - 1;
		index = partition(numbers, start, end);
		while (index != midIndex)
		{
			if (index < midIndex)
			{
				start = index + 1;
				index = partition(numbers,start,end);
			}
			else{

				end = end - 1;
				index = partition(numbers, start, end);
			}
		}
		int result = numbers[midIndex];
		if (!isMoreThanHalf(numbers, result))
			return 0;
		return result;
	}
    //快排中用于找分界点的函数
	int partition(vector<int> numbers, int left, int right){
		int pivot = numbers[left];
		int start = left;
		int end = right;
		while (start != end){
			while (start < end && numbers[end] > pivot)
				end--;
			while (start < end && numbers[start] <= pivot)
				start++;
			if (start < end)
				swap(numbers[start], numbers[end]);
		}
		swap(numbers[left], numbers[start]);
		return start;
	}
	void swap(int &a, int &b){
		int temp = a;
		a = b;
		b = temp;
	}
    //检测数组中数是否满足题目中交代的"数组中有一个数字出现的次数超过数组长度的一半"条件
	bool isMoreThanHalf(vector<int> numbers, int num){
		int times = 0;
		bool isMoreThanHalf = true;
		for (int i = 0; i < numbers.size(); i++){
			if (numbers[i] == num)
				times++;
		}
		if (times * 2 <= numbers.size()){
			isMoreThanHalf = false;
		}
		return isMoreThanHalf;
	}
};
```

方法四：如果有符合条件的数字，则它出现的次数比其他所有数字出现的次数和还要多。**在遍历数组时保存两个值：一是数组中一个数字，一是次数。遍历下一个数字时，若它与之前保存的数字相同，则次数加1，否则次数减1；若次数为0，则保存下一个数字，并将次数置为1。遍历结束后，所保存的数字即为所求。**然后再判断它是否符合条件即可。

```c++
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers)
    {
        if(numbers.empty()) return 0;         
        // 遍历每个元素，并记录次数；若与前一个元素相同，则次数加1，否则次数减1
        int result = numbers[0];
        int times = 1; // 次数
         
        for(int i=1;i<numbers.size();++i)
        {
            if(times == 0)  //每次加减times之前都要判断下是否在遍历上一个元素的时候置为0
            {  
                result = numbers[i];
                times = 1;
                continue;
            }
            if(numbers[i] == result)
                ++times; // 相同则加1
            else
                --times; // 不同则减1               
        }    
        // 检测所给的数组是否存在这样一个数
        times = 0;
        for(int i=0;i<numbers.size();++i)
        {
            if(numbers[i] == result) ++times;
        }        
        return (times > numbers.size()/2) ? result : 0;
    }
};
```

## 最小的K个数

**题目描述**

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。

**思路：**

可以采用快速排序，也可以采用堆排序(尤其适合动态的排序和海量数据排序)

下面提供堆排序代码，采用STL中的堆排序，可参考：[10.堆heap.note](note://1CC6153444B54CF39D58B77F8667A677)

```c++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int len = input.size();
        if(len <=0 || k>len)
            return vector<int>();
        vector<int> res(k+1);  
/*将input中元素全部拷贝到res开始的位置中,注意拷贝的区间为input.begin() ~ input.end()的左闭右开的区间*/
        res.assign(input.begin(),input.begin()+k);
        //实际编程需要包含头文件<algorithm>建堆,默认创建的是最大堆，堆顶为首元素，若要创建最小堆，第三个参数为greater<int>()
        make_heap(res.begin(),res.end());
        for(int i=k;i<len;i++){
            if(input[i]<res[0]){
                res.push_back(input[i]);
                push_heap(res.begin(),res.end());
                
                pop_heap(res.begin(),res.end());
                res.pop_back();
            }
        }
        return res;
    }
};
```

或者直接采用priority_queue

```c++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int len = input.size();
        if(len <=0 ||k<=0 || k > len)  //注意K的范围，之前一直编译不过，就是因为漏掉了k<=0
            return vector<int>();
        priority_queue<int> heap;
        for(int i=0;i<k;i++)
            heap.push(input[i]);
        for(int i=k;i<len;i++){
            if(input[i] < heap.top()){
                heap.pop();
                heap.push(input[i]);
            }
        }
        vector<int> result(k);
        for(int i=k-1;i>=0;i--){
            result[i] = heap.top();
            heap.pop();
        }
        return result;
    }
};
```

## 在排序数组中查找数字

**题目描述**

统计一个数字在排序数组中出现的次数。

思路1：**直接用二分法找出要找的数字K**，然后通过两个while()循环，分别朝前和后扫描，得到K的个数；  **但是当K的个数较多时，前后扫描相当于顺序查找，时间复杂度仍然很高。**



思路二：用二分法，采用递归的方式，分别找到第一个K的下标和，最后一个K的下标，然后根据两个下标的差得出K的个数，代码如下：

```c++
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        int count =0;
        if(data.size() <=0)
            return 0;
        int first = getFirstK(data,k,0,data.size()-1);
        int last = getLastK(data,k,0,data.size()-1);
        if(first==-1 || last==-1)
            return 0;
        return last-first+1;
    }
    int getFirstK(vector<int> data,int k,int start,int end){
        if (start > end)
            return -1;     //数组中不包含数字，返回-1
        int mid = (start+end)>>1;
        if(data[mid]==k){
            if((mid>0 && data[mid-1] != k)||mid==0)
                return mid;
            else
                end = mid - 1;
        }
        else if(data[mid]>k)
            end = mid-1;
        else
            start = mid+1;
        return getFirstK(data,k,start,end);
    }
    int getLastK(vector<int> data,int k,int start,int end){
        if (start > end)
            return -1;    //数组中不包含数字，返回-1
        int mid = (start+end)>>1;
        if(data[mid]==k){
            if((mid < data.size()-1 && data[mid+1] != k)||mid==data.size()-1)
                return mid;
            else
                start = mid + 1;
        }
        else if(data[mid]>k)
            end = mid-1;
        else
            start = mid+1;
        return getLastK(data,k,start,end);
    }
};
```

思路三：和思路二类似，不采用递归的方式，直接用二分法，注意易错点！！！

```c++
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        if(data.size() == 0)
            return 0;
        int left = firstk(data,k);
        int right = lastk(data,k);
        if(left == -1 || right == -1)
            return 0;
        return right-left+1;
    }
    int firstk(vector<int> data,int k){
        int left = 0;
        int right = data.size()-1;
        int mid = (left+right)/2;
        while(left <= right){
            if(data[mid] == k){
                if((mid-1>=left && data[mid-1]!= k) || mid==left)
                    return mid;
//！！！！犯错处，当mid位和mid-1位均等于k，取right=mid-1,不然可能出现死循环。
                right = mid-1;
            }
            else if(data[mid] <k)
                left = mid+1;
            else
                right = mid-1;
            mid = (left+right)/2;
        }
        return -1;
    }
    int lastk(vector<int> data,int k){
        int left = 0;
        int right = data.size()-1;
        int mid = (left+right)/2;
        while(left <= right){
            if(data[mid] == k){
                if((mid+1<=right && data[mid+1]!=k) || mid==right)
                    return mid;
//！！！！犯错处，当mid位和mid+1位均等于k，取left=mid+1,不然可能出现死循环。
                left = mid+1;  
            }
            else if(data[mid] <k)
                left = mid+1;
            else
                right = mid-1;
            mid = (left+right)/2;
        }
        return -1;
    }
};
```

思路四：(大神思路)

由于data中都是整数，所以可以稍微变一下，不是搜索k的两个位置，而是搜索**k-0.5**和**k+0.5**

//这两个数应该插入的位置，然后相减即可。

```c++
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        return biSearch(data, k+0.5) - biSearch(data, k-0.5) ;
    }
private:
    int biSearch(const vector<int> & data, double num){
        int s = 0, e = data.size()-1;     
        while(s <= e){
            int mid = (e - s)/2 + s;
            if(data[mid] < num)
                s = mid + 1;
            else if(data[mid] > num)
                e = mid - 1;
        }
        return s;
    }
};
```

## 数组中只出现一次的两个数字

**题目描述**

一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。



**知识补充：**

[异或的性质及运用](https://www.cnblogs.com/suoloveyou/archive/2012/04/25/2470292.html)

 异或是一种基于二进制的位运算，用符号XOR或者 ^ 表示，其运算法则是对运算符两侧数的每一个二进制位，同值取0，异值取1。它与布尔运算的区别在于，当运算符两侧均为1时，布尔运算的结果为1，异或运算的结果为0。

简单理解就是不进位加法，如1+1=0，,0+0=0,1+0=1。

性质

**1、交换律**

**2、****结合律（即(a^b)^c == a^(b^c)）  重点！！！**

**3、对于任何数x，都有x^x=0，x^0=x**

**4、****自反性 A XOR B XOR B = A xor  0 = A**



**思路：**

要求时间复杂度是O(n)，空间复杂度是O(1)。

大家首先想到的是顺序扫描法，但是这种方法的时间复杂度是O（n^2）。接着大家又会考虑用哈希表的方法，但是空间复杂度不是O（1）。



我们知道异或的一个性质是：**任何一个数字异或它自己都等于0**。也就是说，如果我们从头到尾依次异或数组中的每一个数字，那么最终的结果刚好是那个只出现一次的数字。比如数组{4,5,5}，我们先用数组中的第一个元素4（二进制形式：0100）和数组中的第二个元素5（二进制形式：0101）进行异或操作，0100和0101异或得到0001，用这个得到的元素与数组中的三个元素5（二进制形式：0101）进行异或操作，0001和0101异或得到0100，正好是结果数字4。这是因为数组中相同的元素异或是为0的，**因此就只剩下那个不成对的孤苦伶仃元素。**

现在好了，我们已经知道了如何找到一个数组中找到一个只出现一次的数字，那么我们如何在一个数组中找到两个只出现一次的数字呢？如果，我们可以将原始数组分成两个子数组，使得每个子数组包含一个只出现一次的数字，而其他数字都成对出现。这样，我们就可以用上述方法找到那个**孤苦伶仃的元素。**

我们还是从头到尾一次异或数组中的每一个数字，那么最终得到的结果就是两个只出现一次的数组的异或结果。**因为其他数字都出现了两次，在异或中全部抵消了。**由于两个数字肯定不一样，那么异或的结果肯定不为0，也就是说这个结果数组的二进制表示至少有一个位为1。我们在结果数组中找到第一个为1的位的位置，记为第n位。现在我们以第n位是不是1为标准把元数组中的数字分成两个子数组，第一个子数组中每个数字的第n位都是1，而第二个子数组中每个数字的第n位都是0。



举例：{2,4,3,6,3,2,5,5}

我们依次对数组中的每个数字做异或运行之后，得到的结果用二进制表示是0010。异或得到结果中的倒数第二位是1，于是我们根据数字的倒数第二位是不是1分为两个子数组。第一个子数组{2,3,6,3,2}中所有数字的倒数第二位都是1，而第二个子数组{4,5,5}中所有数字的倒数第二位都是0。接下来只要分别两个子数组求异或，就能找到第一个子数组中只出现一次的数字是6，而第二个子数组中只出现一次的数字是4。



**思路总结：**

本文思路有两大亮点：【1】一个整数数组中如果除了其中一个数以外，其他数都出现了两次，将这些数相异或得到的是这个单一的数。【2】如何将两个单一的数字分到两批数中，将所有数异或，得到的结果其实就是两个单一数字的异或结果，根据其中某一位是否为1将两个数字分开，其他数字也根据这个标准分成了两批。   最终分成的两批数字中，每一批数字中，只有一个单一数字。

```c++
class Solution {
public:
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
        int len = data.size();
        if(len < 2)
            return;
        //对原始数组每个元素依次求异或(异或为位运算)
        int temp=0;
        for(int i=0;i<len;i++)
            temp ^= data[i];
        int indexOf1 = rfindFirst1(temp);
        *num1 =0;
        *num2 =0;
        for(int i=0;i<len;i++){
            if(isBit1(data[i],indexOf1))
                *num1 ^=data[i];
            else
                *num2 ^=data[i];
        }
    }
private:
    //从右找到某个数二进制形式下第一次出现1的下标
    int rfindFirst1(int num){
        int bit1 = 1;
        int index =0;
        while((num & bit1)== 0 && (index < 8*sizeof(int))){
            bit1 = bit1<<1;
            index++;
        }
        return index;
    }
    //判断某个数index位是否为1
    bool isBit1(int num,int index){
        num = num>>index;
        return (num & 1);
    }
};
```

二刷的时候，最终对每个数移位的时候不是通过对该数移动、位之后再判断其是否为1，而是直接通过位与，直接判断相应位是否为1，**但是遇见了个大坑！！！**

**直接写 a & flag != 0;会判断错误**

**经验教训！！！，该多加个括号的时候不要吝啬！！！！**

```c++
class Solution {
public:
	void FindNumsAppearOnce(vector<int> data, int* num1, int *num2) {
		int len = data.size();
		if (len < 2)
			return;
		int temp = 0;
		for (int i = 0; i < len; i++) {
			temp ^= data[i];
		}
		int flag = 1;
		while ((temp & flag) == 0) {
			flag = flag << 1;
		}
		*num1 = 0;
		*num2 = 0;
		for (int i = 0; i < len; i++) {
			//bool aa = isfalag1(data[i], flag);
			if (isfalag1(data[i], flag)) {
				*num1 ^= data[i];
				cout << "第1组" << data[i] << endl;
			}
				
			else {
				*num2 ^= data[i];
				cout << "第2组" << data[i] << endl;
			}
				
		}
	}
    //通过与flag相位与，判断每个数相对于temp中从右到左的第一个为1的位是否为1
	bool isfalag1(int a, int flag) {
		if ((a & flag) != 0) //大坑！！！直接写 a & flag != 0;会判断错误
			return true;
		else
			return false;
	}
};
```

## 和为S的连续正数序列

**题目描述**

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

**输出描述:**

输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

有了解决前面问题的经验，我们也考虑用两个数small和big分别表示序列的最小值和最大值。首先把small初始化为1，big初始化为2。如果从small到big的序列的和大于s，则可以从序列中去掉较小的值，也就是增大small的值。如果从small到big的序列的和小于s，则可以增大big，让这个序列包含更多的数字。因为这个序列至少要有两个数字，我们一直增加small到（1+s）/2为止。

{asset_img 05.png}

```c++
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int> > result;
        if(sum <3)
            return result;
        int left = 1;
        int right = 2;
        int mid = (1+sum)/2;
        while(left < mid){
            if(sum == calculate(left,right)){
                vector<int> temp;
                for(int i=left;i<=right;i++)
                    temp.push_back(i);
                result.push_back(temp);
                right++;
            }
            else if(sum > calculate(left,right))
                right++;
            else
                left++;
        }
        return result;
    }
    long calculate(int a,int b){
        long sum=0;
        for(int i=a;i<=b;i++)
            sum += i;
        return sum;
    }
};
```

## 和为S的两个数字

**题目描述**

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

**输出描述:**

对应每个测试案例，输出两个数，小的先输出。

思路：

方法1：(耗时长)这个问题，很多人都能立即想到O（n2）的方法，也就是先在数组中固定一个数字，再依次判断数组中其余的n-1个数字与它的和是不是等于s。

方法2：我们以数组{1，2，4，7，11，15}及期待的和15为例详细分析一下这个过程。首先定义两个指针，第一个指针指向数组的第一个（最小的）数字1，第二个指针指向数组的最后一个（最大的）数字15。这两个数字的和16大于15，因此我们把第二个指针向前移动一个数字，让它指向11。这时候两个数字1与11的和是12，小于15。接下来我们把第一个指针向后移动一个数字指向2，此时两个数字2与11的和是13，还是小于15。我们再次向后移动第一个指针，让它指向数字4。数字4与11的和是15，正是我们期待的结果。表6.1总结了在数组{1，2，4，7，11，15}中查找和为15的数对的过程。

{asset_img 06.png}

```c++
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        int len = array.size();
        vector<int> result;
        if(len <2)
            return result;
        int left = 0;
        int right = len-1;
        long minMulti = array[right]*array[right];
        
        while(left < right){
            if(array[left]+array[right]==sum){
                if(array[left] * array[right] < minMulti){
                    minMulti = array[left] * array[right];
                    if(result.size() == 0){
                        result.push_back(array[left]);
                        result.push_back(array[right]);
                    }
                    else{
                        result.pop_back();
                        result.pop_back();
                        result.push_back(array[left]);
                        result.push_back(array[right]);
                    }
                }
                left++;
                right--;
            }
            else if(array[left]+array[right] < sum)
                left++;
            else if(array[left]+array[right] > sum)
                right--;
        }
        return result;
    }
};
```

## 扑克牌顺子

**题目：**

从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2~10为数字本身，A为1，J为11，Q为12，K为13，而大、小王可以看成任意数字。在本题中看作0。

**思路：**

判断5个数字是不是连续的。

第一步：排序；

第二步：统计0的个数，统计间隙的个数，**注意：**统计间隙的时候不能和0作运算，非0数字不能重复；

第三步:间隙数大于0个数返回false，间隙数小于等于0的个数返回true。

```c++
class Solution {
public:
    bool IsContinuous( vector<int> numbers ) {
        int len = numbers.size();
        if(len != 5)
            return false;
        sort(numbers.begin(),numbers.end());
        int zeronum = 0;
        for(int i=0;i<len;i++){  //统计0的个数
            if(numbers[i]==0)
                zeronum++;
        }
        int pre=numbers[0];
        int diffCount = 0;
        for(int i=1;i<len;i++){
            int cur = numbers[i];
            if(cur != 0&& pre !=0){  //要考虑，计算间隙的时候，不能和0作运算，如[3,0,2,6,4]
                if(cur == pre)   //要考虑数相等的情况，如[1,0,0,1,0]
                    return false;
                diffCount += (cur-pre-1);
            }
            pre = cur;
        }
        return zeronum>=diffCount;
    }
};
```

## 数组中重复的数字

题目描述(数组)

在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。



**一、可以改变数组**

【方法1】先把输入的数组排序(sort())，然后遍历数组，找出重复元素，时间复杂度为O(nlogn);

【方法2】**时间复杂度为O(n),空间复杂度为O(1)**  

思路：从头到尾依次扫描数组元素，当扫描到第i个元素m时，当m等于i时，继续遍历下一元素，当不等于i时，则拿他与第m个数n比较，如果m=n，则找到重复元素，返回true,否则交换两元素...

```c++
class Solution {
public:
    bool duplicate(int numbers[], int length, int* duplication) {
        //如果数组为空，或者输入的长度小于等于0
        if(numbers == nullptr||length <= 0){
            return false;
        }
        //确保数组中每个数在0~length-1之间
        for(int i=0;i<length;i++){
            if(numbers[i]<0 || numbers[i] > length-1)
                return false;
        }
        for(int i = 0; i < length; i++){
            while(i != numbers[i])
            {
                if(numbers[i] == numbers[numbers[i]])
                {
                    *duplication = numbers[i];
                    return true;
                }
                //交换第i和第numbers[i]个数
                int temp = numbers[i];
                numbers[i] = numbers[temp];
                numbers[temp] = temp;//注意该句千万不要写成numbers[numbers[i]] = temp;
            }
        }
        return false;
    }
};
```

**二、不能改变数组**

【方法1】创建一个长度为length的辅助数组，然后逐一把原数组的每一个数字复制到辅助数组，这样很容易找到重复数字，时间复杂度O(N),空间复杂度O(N)。

【方法2】用STL中的set,将数组中的数，插入set,当set的大小不变的时候，所插入的数字即为重复数字。

## 构建乘积数组

**题目描述**

给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

**思路：**

求B[i]的时候分为两步，先求左侧所有的A相乘，再求右侧的所有的A相乘，最终将两者相乘即得相应的B。具体通过两次循环，累乘得到不同的B[i]，代码如下：

```c++
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        int len = A.size();
        vector<int> B(len);
        if(len == 0)
            return B;
//总结：第0位和第n-1位为特殊情况，其对应左侧和右侧乘积均当做1，for循环都从第二位和倒数第二位起
        B[0] = 1;//此处的B[0]代表第0位前A[i]的乘积，初始化为1
        for(int i=1;i<len;i++){  //累乘求第1位到第n-1位前面所有A[i]的乘积
            B[i] = B[i-1]*A[i-1];
        }
        int temp = 1; //此处的temp代表最后1位后面A[i]的乘积，对应的是最后一位，所以初始化为1
        for(int i=len-2;i>=0;i--){//累乘求第n-2位到第0位前面所有A[i]的乘积
            temp *=A[i+1];
            B[i] *= temp;
        }
        return B;
    }
};
```

