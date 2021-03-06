# 39.平衡二叉树


## 39.平衡二叉树

### 题目描述  

- 输入一棵二叉树，判断该二叉树是否是平衡二叉树。

&nbsp;

### 解题思路  

**平衡二叉树**：平衡二叉树（Balanced Binary Tree）具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。平衡二叉树的常用实现方法有[红黑树](https://baike.baidu.com/item/红黑树/2413209)、[AVL](https://baike.baidu.com/item/AVL/7543015)、[替罪羊树](https://baike.baidu.com/item/替罪羊树/13859070)、[Treap](https://baike.baidu.com/item/Treap)、[伸展树](https://baike.baidu.com/item/伸展树/7003945)等。 最小二叉平衡树的节点的公式如下$ F(n)=F(n-1)+F(n-2)+1 $这个类似于一个递归的[数列](https://baike.baidu.com/item/数列/731531)，可以参考[Fibonacci数列](https://baike.baidu.com/item/Fibonacci数列)，1是根节点，$F(n-1)$是左子树的节点数量，$F(n-2)$是右子树的节点数量。（来自百度百科）  

下面的思路来自《剑指offer》。

- 思路一，前序遍历向下递归求深度。利用《38.二叉树的深度》中求深度的函数，使用前序遍历向下递归时，依次对每个结点的左右子结点求深度，判断该结点作为根节点的树是否是平衡的。但是这种方法会导致求深度时很多结点被重复遍历。
- 思路二，后序遍历向上递归动态规划，每个结点只遍历一次。使用后序遍历的方法，先判断左右子树是否平衡，在判断的时候会传回左右子树的深度，然后向上返回自己是否平衡以及自己作为根结点的树的深度。


&nbsp;

### 代码 

- 思路二，后序遍历向上递归动态规划，C++

```c++
class Solution {
private:
    bool isBalanced(TreeNode* pRoot, int* depth){
        if(pRoot == NULL) {
            *depth = 0;
            return true;
        }
        int lDepth, rDepth;
        if(isBalanced(pRoot->left, &lDepth) && isBalanced(pRoot->right, &rDepth)){
            int diff = lDepth - rDepth;
            if(diff>=-1 && diff<=1){
                *depth = (lDepth>rDepth) ? (lDepth+1) : (rDepth+1);
                return true;
            }
        }
        return false;
    }
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        int depth;
        return isBalanced(pRoot, &depth);
    }
};
```




