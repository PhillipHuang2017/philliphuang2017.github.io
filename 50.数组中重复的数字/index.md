# 50.数组中重复的数字


## 50.数组中重复的数字

### 题目描述  

- 在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

&nbsp;

### 解题思路  

- 思路一，哈希表，空间换时间。创建一个长度为`n`的哈希表，里面存储从`0~n-1`每个数字出现的次数（或者存储`bool`值，表示这个数是否出现过，但占用空间都一样，因为`bool`也是用`int`存储的），如果某一次哪个数字出现次数大于1了就返回。需要大小为`n`的额外存储空间，因此空间复杂度为$O(n)$，最多遍历`n`个数，因此时间复杂度为$O(n)$。
- 思路二，排序后遍历。首先对数组进行排序，然后依次遍历过去，看相邻数字是否相同。快速排序算法的时间复杂度为$O(nlogn)$，遍历最多`n`次，因此总的时间复杂度为$O(nlogn)+O(n)$。
- 思路三（推荐），输入数组自身作为标志数组。题目中说了数字的大小为`0~n-1`，因此用原数组作为哈希表即可，每遍历一个数就把该数作为下标的位置的数`+n`，下次再遍历到相同数字时会发现这个位置的数已经大于`n`了。不需要额外空间，时间复杂度为$O(n)$。


&nbsp;

### 代码 

- 思路三（推荐），输入数组自身作为标志数组。

```c
class Solution {
public:
    // Parameters:
    //        numbers:     an array of integers
    //        length:      the length of array numbers
    //        duplication: (Output) the duplicated number in the array number
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    bool duplicate(int numbers[], int length, int* duplication) {
        if(numbers==NULL || length<2) return false;
        for(int i=0; i<length; i++){
            int index = numbers[i]%length;
            if(numbers[index] >= length){
                *duplication = index;
                return true;
            }
            numbers[index] += length;
        }
        return false;
    }
};
```




