# 37.数字在排序数组中出现的次数


## 37.数字在排序数组中出现的次数

### 题目描述  

- 统计一个数字在排序数组中出现的次数。

&nbsp;

### 解题思路  

下面的思路来自《剑指offer》。

- 思路一，从头到尾遍历统计，复杂度$O(n)$。
- 思路二，折半查找，前后统计。既然是排序的数组，那就比较好办了，只需要找到这个数的位置，然后向前和向后统计一下数量就行，复杂度$O(n)$。
- 思路三，找到第一个k和最后一个k的位置。利用二分查找法查找第一个k的位置，然后查找最后一个k的位置，有了两个位置就可以知道数字的数量了。复杂度$O(logn)$。


&nbsp;

### 代码 

- 思路三，找到第一个k和最后一个k的位置。

```c++
class Solution {
private:
    int GetFirstK(vector<int> &data ,int k){
        int length = data.size();
        int low=0, high=length-1;
        while(low<=high){
            int mid = (low+high)>>1;
            if(data[mid]==k){
                if(mid>0 && data[mid-1]!=k || mid==0) return mid;
                else high = mid-1;  // 左边还等于k的话往左边找
            }
            else if(data[mid]>k) high = mid-1;
            else low = mid+1;
        }
        return -1;
    }
    
    int GetLastK(vector<int> &data ,int k){
        int length = data.size();
        int low=0, high=length-1;
        while(low<=high){
            int mid = (low+high)>>1;
            if(data[mid]==k){
                if(mid<length-1 && data[mid+1]!=k || mid==length-1) return mid;
                else low = mid+1;  // 右边还等于k的话往右边找
            }
            else if(data[mid]>k) high = mid-1;
            else low = mid+1;
        }
        return -1;
    }
public:
    int GetNumberOfK(vector<int> data ,int k) {
        int firstK = GetFirstK(data, k);
        if(firstK == -1) return 0;
        int lastK = GetLastK(data, k);
        return lastK-firstK+1;
    }
};
```




