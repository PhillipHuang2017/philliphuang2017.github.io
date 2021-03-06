# 30.连续子数组的最大和


## 30.连续子数组的最大和

### 题目描述  

- HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)。

&nbsp;

### 解题思路  

下面的思路来自《剑指offer》

- 思路一，分析规律。首先将数组的第一个数作为`curSum`以及`maxSum`，然后依次遍历后面的数，如果`curSum<=0`的话，那么一定是从新的这个数开始加会更大（如果新的数是负数，那么加进去就更小了，如果新的数是正数，那么从新的数开始加就已经比之前的`curSum`更大了），因此如果`curSum<=0`就将其直接赋值为新的数，从这个新的数开始加；如果`curSum>0`的话就把新的数加进去。然后看一下是否需要更新`maxSum`。当遍历结束的时候`maxSum`就是连续子序列的最大和。

- 思路二，动态规划法。还是上面的规律，如果$f(i)$表示以下标$i$结尾的子序列的最大和，那么可以得到下面的公式：
$$
  f(i) = \begin{cases}
array[i] &\text{if }i=0 \text{ or }f(i-1)\leqslant0 \\
  f(i-1)+array[i] &\text{if }f(i-1)\gt0 \\
  \end{cases}
  $$
  写出来的代码和思路一是一样的，$f(i)$就相当于`curSum`，$max(f(i))$就是`maxSum`。


&nbsp;

### 代码 

- 思路一，两种思路的代码是一样的。

```c++
class Solution {
public:
    int FindGreatestSumOfSubArray(vector<int> array) {
        int length = array.size();
        if(length==0) return 0;
        if(length==1) return array[0];
        
        int curSum = array[0];
        int maxSum = array[0];
        for(int i=1;i<length;i++){
            if(curSum<=0) curSum = array[i];
            else curSum += array[i];
            if(curSum>maxSum) maxSum = curSum;
        }
        return maxSum;
    }
};
```


