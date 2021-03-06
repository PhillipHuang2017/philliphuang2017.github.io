# 28.数组中出现次数超过一半的数字


## 28.数组中出现次数超过一半的数字

### 题目描述  

- 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组`{1,2,3,2,2,2,5,4,2}`。由于数字`2`在数组中出现了5次，超过数组长度的一半，因此输出`2`。如果不存在则输出`0`。

&nbsp;

### 解题思路  

下面的思路来自《剑指offer》，思路的名字是我自己起的。

- 思路一，排序后取中位数。首先进行快速排序，如果存在出现次数超过数组长度一半的数，那么这个数一定是排序以后最中间的数，即输入数组的中位数。然后再判断这个数字出现的次数是否超过数组长度的一半即可。但是这种方法由于使用了排序算法，因此速度会受到限制。
- 思路二，消消乐。如果一个数出现的次数超过数组长度的一半，那么这个数出现的次数一定比其他数字的次数加起来还要多。因此可以用一个变量保存数字，另一个变量保存次数，如果后一个数和保存的数一样，次数就+1，如果不一样，次数就-1，如果次数为0，则保存下一个数，并把次数设为1。到最后只需要判断保存的这个数出现的次数是否超过数组长度的一半即可。


&nbsp;

### 代码 

- 思路一，排序后取中位数

```c++
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        int length = numbers.size();
        if(length==0) return 0;
        // 调用标准库的快速排序算法，面试的时候可能要自己写
        sort(numbers.begin(), numbers.end());
        int middle = numbers[length>>1];
        int times=0;
        // 下面这种统计次数的方法可以不用判断后面的数字，但仅限于先排序的方法
        int i=0;
        while(numbers[i] != middle){
            i++;
        }
        for(; i<length; i++){
            if(numbers[i] != middle) break;
            times++;
        }
        return (times > length>>1) ? middle:0;
    }
};
```

- 思路二，消消乐

```c++
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        int length = numbers.size();
        if(length==0) return 0;
        
        int result = numbers[0];
        int times=1;
        for(int i=1; i<length; i++){
            if(times==0) {
                result = numbers[i];
                times = 1;
                continue;
            }
            if(numbers[i]==result) times++;
            else times--;
        }
        times = 0;
        for(int i=0; i<length; i++)
            if(numbers[i]==result) times++;
        
        return (times > length>>1) ? result:0;
    }
};
```




