# 23.二叉搜索树的后序遍历序列


## 23.二叉搜索树的后序遍历序列

### 题目描述  

- 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes, 否则输出No。假设输入的数组的任意两个数字都互不相同。

&nbsp;


### 解题思路  

- 这种判断是不是某种遍历结果的题目一般都是通过依次划分左右子树，然后递归下去的方式。要先想这种遍历方式的最显著特点，后序遍历的话，序列的末尾一定是根，然后这又是二叉搜索树（BST），因此整个序列中，第一个比根大的结点一定是右子树中打印的第一个结点`i`，结点`i`左边的所有元素都是左子树，都需要比根小（已经满足），结点`i`到根节点之间的元素都是右子树，都需要比根大。如果满足这个规则的话再划分左右子树，对左右子树再进行递归判断。
- 牛客用户[马客(Mark)](https://www.nowcoder.com/profile/899694)的题解，简洁明了：BST的后序序列的合法序列是，对于一个序列S，最后一个元素是x （也就是根），如果去掉最后一个元素的序列为T，那么T满足：T可以分成两段，前一段（左子树）小于x，后一段（右子树）大于x，且这两段（子树）都是合法的后序序列。完美的递归定义 : ) 。[链接](https://www.nowcoder.com/profile/8378038/codeBookDetail?submissionId=17463800)

&nbsp;

### 代码 

```c++
class Solution {
private:
    bool judge(vector<int> sequence, int begin, int end){
        // 只剩两个数时其实就不用判断了，三个数的时候还是要判断的
        if(begin >= end-1) return true;  
        int i = 0;
        while(sequence[i]<sequence[end]) i++;
        for(int j=i+1; j<end; j++){
            if(sequence[j]<sequence[end]) return false;
        }
        // 左右子树的结果
        bool l_result = judge(sequence, begin, i-1);
        bool r_result = judge(sequence, i, end-1);
        return l_result && r_result;
    }
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        int size = sequence.size();
        if(size==0) return false;
        return judge(sequence, 0, size-1);
    }
};
```

