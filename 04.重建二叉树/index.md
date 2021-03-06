# 04.重建二叉树


## 4.重建二叉树
### 题目描述  
输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。   
假设输入的前序遍历和中序遍历的结果中都不含重复的数字。   
例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。    




### 分析
**前序遍历**：中左右，第一个为根节点，后面左子树所有值输出完以后的下一个值为右子树的根节点，依次递归。
**中序遍历**：左中右，根节点左边全部为左子树的内容，右边全部为右子树的内容，依次递归。

假设前序遍历序列为数组pre，中序遍历序列为数组vin 
首先根据上面的规律找到根节点在中序遍历序列vin中的下标，得到根节点，然后将vin中根节点的左边序列起始和结尾下标（左子树结点，假设有nl个）和pre中根节点后面nl个结点的起始和结尾下标（左子树结点）递归调用函数自身查找左子树的根节点，右子树也一样。   




### c++代码
```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* R(vector<int> p, int p_beg, int p_end, vector<int> v, int v_beg, int v_end) {
        if(p_beg>=p_end || v_beg>=v_end) return NULL;
        int pivot;
        for(pivot=v_beg; pivot<v_end; pivot++){
            if(v[pivot] == p[p_beg]) break;
        }
        TreeNode *root = new TreeNode(p[p_beg]);
        root->left = R(p, p_beg+1, p_beg+(pivot-v_beg)+1, v, v_beg, pivot);
        root->right = R(p, p_beg+(pivot-v_beg)+1, p_end, v, pivot+1, v_end);
        return root;
            
    }
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        return R(pre, 0, pre.size(), vin, 0, vin.size());
    }
};

```


