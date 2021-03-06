# 24.二叉树中和为某一值的路径


## 24.二叉树中和为某一值的路径

### 题目描述  

- 输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

&nbsp;


### 解题思路  

- 思路一，递归，直接看代码可能更好理解一点，很巧妙。创建一个结果嵌套列表全局变量，和一个临时列表全局变量。从根节点一路往下递归，每次将结点加入临时列表，到底部时如果总和满足要求就将临时列表加入结果列表。每次返回之前将压入的结点弹出。

&nbsp;

### 代码 

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
private:
    vector<vector<int> > result;
    vector<int> temp;
    
public:
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        if(root==NULL) return result;
        temp.push_back(root->val);
        if(expectNumber - root->val == 0 && !root->left && !root->right){
            result.push_back(temp);
        }
        FindPath(root->left, expectNumber - root->val);
        FindPath(root->right, expectNumber-root->val);
        temp.pop_back();
        return result;
    }
};
```

