# 35.数组中的逆序对


## 35.数组中的逆序对

### 题目描述  

- 在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007  

  输入描述:  

  > 题目保证输入的数组中没有的相同的数字。
  >
  > 数据范围：
  >
  > ​	对于%50的数据,size<=10^4；
  >
  > ​	对于%75的数据,size<=10^5；
  >
  > ​	对于%100的数据,size<=2*10^5

  **示例1**

  输入：

  ```
  1,2,3,4,5,6,7,0
  ```

  输出：

  ```
  7
  ```

&nbsp;

### 解题思路  

下面的思路来自《剑指offer》。

- 思路一，笨办法。每个数都和后面的所有数进行比较，复杂度为$O(n^{2})$。
- 思路二，归并排序法。采用归并排序法的思路，假设左右两边的数组已经分别都是有序的，设置两个指针`p1`和`p2`分别指向两个数组的最后一个位置，比较一下`p1`和`p2`位置的大小，如果`p1`位置的数更大的话，说明`p1`位置的数和后面序列剩余的所有的数都构成逆序对，然后把大的那个数放入临时数组的最后，相应指针前移。如果是`p2`位置的数更大的话，直接把`p2`位置的数放入临时数组，指针前移，逆序对数量不变。
- 最后不要忘了按题目要求对1000000007取模（取余数），估计是结果太大，返回的int放不下。


&nbsp;

### 代码 

- 思路二，归并排序法。

```c++
class Solution {
private:
    unsigned long MergeSortCount(vector<int> &data, int start, int end, vector<int> &temp){
        if(start==end) return 0;
        int middle = (start+end)>>1;
        unsigned long leftPairs = MergeSortCount(data, start, middle, temp);
        unsigned long rightPairs = MergeSortCount(data, middle+1, end, temp);
        unsigned long pairsNum = leftPairs + rightPairs;
        int pL = middle, pR = end, pT = end;
        while(pL>=start && pR>=middle+1){
            if(data[pL]>data[pR]){
                pairsNum += pR-middle;
                temp[pT] = data[pL];
                pT--;
                pL--;
            }
            else{
                temp[pT] = data[pR];
                pT--;
                pR--;
            }
        }
        while(pL>=start){
            temp[pT] = data[pL];
            pT--;
            pL--;
        }
        while(pR>=middle+1){
            temp[pT] = data[pR];
            pT--;
            pR--;
        }
        for(int i=start; i<=end; i++){
            data[i] = temp[i];
        }
        return pairsNum;
    }
public:
    int InversePairs(vector<int> data) {
        int length = data.size();
        if(length==0) return 0;
        vector<int> temp(length);
        unsigned long pairsNum = MergeSortCount(data, 0, length-1, temp);
        return pairsNum%1000000007;
    }
};
```




