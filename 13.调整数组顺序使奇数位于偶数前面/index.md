# 13.调整数组顺序使奇数位于偶数前面


## 13.调整数组顺序使奇数位于偶数前面  

### 题目描述  

- 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。   

### 解题思路  

- 方法一，类似于插入排序，依次检测，遇到奇数就把奇数调到前面去，前面的偶数依次向后移，设置一个记录下一个奇数要插入到哪个位置的变量。

- 方法二，简单清晰但占用内存大，再创建一个数组，先把原数组遍历一遍，遇到奇数就`push_back`到新数组，再遍历一遍原数组，遇到偶数就`push_back`到新数组，最后再把新数组赋值给原数组。甚至可以创建两个数组，一个保存奇数，另一个保存偶数，最后再把偶数的`push_back`到奇数的里面，再赋值给原数组。

- 方法三，类似于冒泡排序，遇到奇偶顺序不对的，就交换位置，最终奇数会慢慢地冒泡到最前面，偶数会慢慢地冒泡到最后面。

  > 冒泡排序：依次比较相邻的两个数的大小，如果不符合就交换位置。
  >
  > 外层循环遍历的次数为数组长度`length-1`，循环变量为`i`，内层循环遍历次数为`length-1-i`，循环变量为`j`，依次比较`j`和`j+1`的大小因为内层循环每执行完一次以后，前面最大的1个数都会沉到最后面，因此最后面的`i`个数字无需再比较，换句话说`i`可以表示已经沉到底部的大数的个数。



### 代码 

- 方法一，类似于插入排序，遇到奇数就调到前面

```c
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        int idx = 0; //下一个奇数要插入到前面的哪个位置
        for(int i = 0; i < array.size(); i++){
            if(array[i]%2==1){
                int temp = array[i]; 
                int j = i;
                while(j > idx){
                    array[j] = array[j-1];
                    j--;
                }
                array[j] = temp;
                idx++;
            }
        }
    }
};
```

- 方法二，创建新的数组，简单清晰，但占内存大

```c
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        vector<int> result;
        int length = array.size()
        for(int i = 0; i < length; i++){
            if(array[i]%2==1){
                result.push_back(array[i]);
            }
        }
        for(int i = 0; i < length; i++){
            if(array[i]%2==0){
                result.push_back(array[i]);
            }
        }
        array = result;
    }
};
```

- 方法三，类似于冒泡排序，奇偶顺序不对就交换位置，每次都会有一个偶数沉到最后面

```c
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        int length = array.size();
        for(int i = 0; i < length-1; i++){
            bool finish = true;
            for(int j = 0; j < length-1-i; j++){
                if(array[j]%2==0 && array[j+1]%2==1){
                    int temp = array[j+1];
                    array[j+1] = array[j];
                    array[j] = temp;
                    finish = false;
                }
            }
            if(finish) return;
        }
    }
};
```




