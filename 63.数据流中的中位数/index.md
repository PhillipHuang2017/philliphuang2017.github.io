# 63.数据流中的中位数


## 63.数据流中的中位数

### 题目描述  

- 如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用`Insert()`方法读取数据流，使用`GetMedian()`方法获取当前读取数据的中位数。

&nbsp;

### 解题思路  

下面的思路来自牛客网题解。  

可以通过大顶堆和小顶堆来解决这个问题，大顶堆放比较小的数，小顶堆放比较大的数，并且要求两个顶堆中数字个数差不大于1。这样的话中位数就是数字多的那个堆的堆顶或者两个堆的堆顶元素的平均数。

构建大顶堆和小顶堆时可以使用`priority_queue`直接构建顶堆，也可以使用`vector`数组结合`make_heap`，`push_heap()`，`pop_heap()`来实现。

（1）`make_heap()`构造堆

`void make_heap(first_pointer,end_pointer,compare_function);`

默认比较函数是(<)，即最大堆。

如`make_heap(vectorA.begin(), bectorA.end(), less<int>)`即可构建最大堆。

函数的作用是将`[first_pointer,end_pointer)`内的元素处理成堆的结构，处理完以后下标为`0`的位置为堆顶。

（2）`push_heap()`添加元素到堆

`void push_heap(first_pointer,end_pointer,compare_function);`

新添加一个元素在末尾，然后调用该函数重新调整堆序。该算法必须是在一个已经满足堆序的条件下。

> 先在`vector`的末尾添加元素，再调用`push_heap`

（3）`pop_heap()`从堆中移出元素

`void pop_heap(first_pointer,end_pointer,compare_function);`

把堆顶元素取出来，放到了数组或者是`vector`的末尾。

要取走，则可以使用底部容器（`vector`）提供的`pop_back()`函数。

> 先调用`pop_heap`再从`vector`中`pop_back`元素


&nbsp;

### 代码 

- 思路一，排序后取中位数，使用`priority_queue`。

```c
class Solution {
private:
    priority_queue<int, vector<int>, less<int>> maxHeap; // 大顶堆存放小数字
    priority_queue<int, vector<int>, greater<int>> minHeap; // 小顶堆存放大数字
public:
    void Insert(int num)
    {
        // 小数字就插入大顶堆，大数字就插入小顶堆，默认插入大顶堆
        if(maxHeap.empty() || num < maxHeap.top()) maxHeap.push(num);
        else minHeap.push(num);
        // 调整两个顶堆的平衡，如果不能平衡的话默认让大顶堆数字更多（因为数字默认插入大顶堆）
        if(maxHeap.size() == minHeap.size()+2) {
            minHeap.push(maxHeap.top());
            maxHeap.pop();
        }
        if(minHeap.size() == maxHeap.size()+1) {
            maxHeap.push(minHeap.top());
            minHeap.pop();
        }
    }

    double GetMedian()
    { 
        // Insert函数中不平衡时默认大顶堆数字更多
        if(maxHeap.size() > minHeap.size()) return maxHeap.top();
        else return double(maxHeap.top() + minHeap.top())/2;
    }
};
```




