# 22.从上往下打印二叉树


## 22.从上往下打印二叉树

### 题目描述  

- 从上往下打印出二叉树的每个节点，同层节点从左至右打印。

&nbsp;


### 解题思路  

- So easy！通过队列就可以实现，先将根结点入队，然后每次从队列取出一个结点并输出，将结点的左右子结点入队。

&nbsp;

### 代码 

- 思路一，也可以在循环外面把根结点入队，然后输出和左右子结点入队的操作就可以在循环中统一完成了。这样代码会简洁一点，但是多了一次入队出队操作。

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
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> result;
        if(root==NULL) return result;
        queue<TreeNode*> outQueue;
        result.push_back(root->val);
        if(root->left) outQueue.push(root->left);
        if(root->right) outQueue.push(root->right);
        while(!outQueue.empty()){
            TreeNode* temp = outQueue.front();
            outQueue.pop();
            result.push_back(temp->val);
            if(temp->left) outQueue.push(temp->left);
            if(temp->right) outQueue.push(temp->right);
        }
        return result;
    }
};
```

