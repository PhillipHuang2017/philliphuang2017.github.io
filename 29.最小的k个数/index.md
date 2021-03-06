# 29.最小的K个数


## 29.最小的K个数

### 题目描述  

- 输入n个整数，找出其中最小的K个数。例如输入`4,5,1,6,2,7,3,8`这8个数字，则最小的`4`个数字是`1,2,3,4,`。

&nbsp;

### 解题思路  

下面的思路来自《剑指offer》

- 思路一，快速排序后取前`k`位，写个快速排序还不是分分钟的事？看一下代码会发现**实际上并不需要完全排序完毕**。这种方法比较快，但是会对输入数组进行修改，如果要求不能修改输入数组的话这种方法就不好用了。

- 思路二，用一个大小为`k`的容器（最大堆或者红黑树），依次读取数据，如果比当前容器最大值小的话，就用这个数替换容器的最大值，并重新计算最大值，否则放弃这个数。最后容器内的k个数就是最小的`k`个数。这个容器可以用STL中的最大堆（根结点的值总是大于其子树中任意结点的值）或者红黑树来实现。这种方法可以不用修改输入数组，适用于海量数据。   

  STL中关于堆的操作：

  > 头文件是`#include <algorithm>`，牛客网的编辑器好像已经包含了该头文件。
  >
  > （1）`make_heap()`构造堆
  >
  > `void make_heap(first_pointer,end_pointer,compare_function);`
  >
  > 默认比较函数是(`<`)，即最大堆。
  >
  > 函数的作用是将`[begin,end)`内的元素处理成堆的结构，此时下标为0的位置为最大值
  >
  >  
  >
  > （2）`push_heap()`添加元素到堆
  >
  > `void push_heap(first_pointer,end_pointer,compare_function);`
  >
  > 新添加一个元素在末尾，然后重新调整堆序。该算法必须是在一个已经满足堆序的条件下。
  >
  > 先在`vector`的末尾添加元素，再调用`push_heap`
  >
  >  
  >
  > （3）`pop_heap()`从堆中移出元素
  >
  > `void pop_heap(first_pointer,end_pointer,compare_function);`
  >
  > 把堆顶元素取出来，放到了数组或者是`vector`的末尾。
  >
  > 要取走，则可以使用底部容器（`vector`）提供的`pop_back()`函数。
  >
  > 先调用`pop_heap`再从`vector`中`pop_back`元素
  >
  >  
  >
  > （4）`sort_heap()`对整个堆排序
  >
  >  `void sort_heap (first_pointer,end_pointer,compare_function);`
  >
  > 排序之后的元素就不再是一个合法的堆了，就变成从小到大排序了，和`sort`函数的效果一样。


&nbsp;

### 代码 

- 思路一，快速排序法

```c++
class Solution {
private:
    int Partition(vector<int> &input, int left, int right){
        int pivot = input[left];
        while(left<right){
            while(right>left && input[right]>=pivot){
                right--;
            }
            input[left] = input[right];
            while(left<right && input[left]<=pivot){
                left++;
            }
            input[right] = input[left];
        }
        input[left] = pivot;
        return left;
    }
    
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int length = input.size();
        if(length==0 || k<=0 || k>length) return vector<int>();
        
        int left = 0;
        int right = length-1;
        int index = Partition(input, left, right);
        while(index!=k-1){
            if(index<k-1){
                left = index+1;
            }
            else{
                right = index-1;
            }
            index = Partition(input, left, right);
        }
        
        vector<int> result(input.begin(), input.begin()+k);
        return result;
    }
};
```

- 思路二，最大堆（如果比较强的话可以自己写堆相关的代码）

```c++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int length = input.size();
        if(length==0 || k<=0 || k>length) return vector<int>();
        
        // 通过数组地址初始化vector数组
        vector<int> result(input.begin(), input.begin()+k);
        // 构造大顶堆，最后一个参数默认就是less<int>()，即大顶堆，因此可以不写
        make_heap(result.begin(), result.end(), less<int>());
        
        for(int i=k; i<length; i++){
            if(input[i]<result[0]){
                pop_heap(result.begin(), result.end(), less<int>());
                result.pop_back();
                result.push_back(input[i]);
                push_heap(result.begin(), result.end(), less<int>());
            }
        }
        sort_heap(result.begin(), result.end(), less<int>());
        return result;
    }
};
```




