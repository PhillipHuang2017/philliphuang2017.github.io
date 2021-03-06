# 06.旋转数组的最小数字


## 6.旋转数组的最小数字  

### 题目描述  
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。   
输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。   
例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。   
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。   


### 解题思路
可以采用二分查找法的思想，但是会受到限制。主要思路如下：   
vector数组为array，当数组是旋转的数组时，最小值其实就是左右两个数组的分界点，目标就是找到这个分界点，假设左边为数组1，最小值及其右边为数组2；     
如果数组是旋转的，则肯定`array[low]>=array[high]`，所以如果进入循环的时候`array[low]<array[high]`，则说明中间这段数组已经不是旋转的了，就可以直接返回`array[low]`（但是得保证上一次循环的时候low或者high没有越界，即没有越过分界点直接跨到对方的数组中，如何保证见下面），循环中具体步骤如下；
- 如果`array[low]>=array[high]`，可以尝试着使用二分查找向中间逼近，`mid = low + (high - low)/2`；   
**情况1**：有可能`mid`这个步子跨的有点大，直接跨到数组2了，即`array[mid]<array[high]`，这时候就该让`high=mid`（不让`high=mid-1`是因为有可能`mid`刚好踩在分界点上）；   
**情况2**：如果`array[mid]==array[high]`则不知道跨到哪边了，因此不能贸然将指针指向`mid`，只能一步一步走，因此`low=low+1`；   
**情况3**： 如果`array[mid]>array[high]`则说明`mid`还在数组1中，所以可以让`low=mid+1`。 
&nbsp;
- 如果`array[low]<array[high]`，则说明low就是分界点，在开头已经说过了，因为上面的情况中`low`和`high`向中间逼近的时候都是尽量保证不越界的，因此一旦`array[low]<array[high]`，必然是`low`走到分界点了。   



### 代码
```c++
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        int size = rotateArray.size();
        if(size == 0){
            return 0;
        }
        int low = 0;
        int high = size - 1;
        int mid;
        while(low < high){
            if(rotateArray[low] < rotateArray[high]){
                return rotateArray[low];
            }
            else{
                mid = low + (high - low)/2;
                if(rotateArray[mid] < rotateArray[high]){
                    high = mid;
                }
                else if(rotateArray[mid] > rotateArray[high]){
                    low = mid + 1;
                }
                else{
                    low += 1;
                }
            }
            
        }
        return rotateArray[low];
    }
};
```
