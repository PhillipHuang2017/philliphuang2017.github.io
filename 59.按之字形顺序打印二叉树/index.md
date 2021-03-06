# 59.按之字形顺序打印二叉树


## 59.按之字形顺序打印二叉树

### 题目描述  

- 请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

&nbsp;

### 解题思路  

下面的思路来自《剑指offer》。

- 思路一，两个栈。第一排先入`stackA`，然后`stackA`中依次出栈的时候每个结点按先左子结点后右子结点的顺序把子结点入`stackB`，然后`stackB`依次出栈的时候按先右子结点后左子结点的顺序把子结点入`stackA`。当两个栈同时为空时结束循环。


&nbsp;

### 代码 

- 思路一，两个栈。

```c
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
class Solution {
public:
    vector<vector<int> > Print(TreeNode* pRoot) {
        vector<vector<int> > result;
        if(pRoot==NULL) return result;
        stack<TreeNode*> stackA;
        stack<TreeNode*> stackB;
        vector<int> line;
        TreeNode* temp;
        
        stackA.push(pRoot);
        while(!stackA.empty() || !stackB.empty()){
            line.clear();
            while(!stackA.empty()){
                temp = stackA.top();
                stackA.pop();
                line.push_back(temp->val);
                if(temp->left) stackB.push(temp->left);
                if(temp->right) stackB.push(temp->right);
            }
            if(!line.empty()) result.push_back(line);
            
            line.clear();
            while(!stackB.empty()){
                temp = stackB.top();
                stackB.pop();
                line.push_back(temp->val);
                if(temp->right) stackA.push(temp->right);
                if(temp->left) stackA.push(temp->left);
            }
            if(!line.empty()) result.push_back(line);
        }
        return result;
    }
};
```




