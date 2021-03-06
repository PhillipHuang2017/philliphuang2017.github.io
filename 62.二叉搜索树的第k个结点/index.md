# 62.二叉搜索树的第k个结点


## 62.二叉搜索树的第k个结点

### 题目描述  

- 给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）  中，按结点数值大小顺序第三小结点的值为4。

&nbsp;

### 解题思路  

仔细想一下，其实这个问题不就是求中序遍历的第`k`个值吗？或者说深度优先搜索的第`k`个值。但是写代码的时候要注意，普通的递归方法中序遍历的话会把整棵树都遍历完，但是没有这个必要，某次递归时如果发现已经找到结果了就直接返回就行。


&nbsp;

### 代码 

- 递归，实现一。

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
private:
    TreeNode* result;
    void dfs(TreeNode* pNode, int &k){
        if(pNode==NULL || result) return;
        dfs(pNode->left, k);
        k--;
        if(k==0){
            result = pNode;
            return;
        }
        dfs(pNode->right, k);
    }
public:
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        result = NULL;
        dfs(pRoot, k);
        return result;
    }
};
```

- 递归，实现二

```C
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
private:
    int count = 0;
public:
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        if(pRoot==NULL) return NULL;
        TreeNode* result;
        result = KthNode(pRoot->left, k);
        if(result) return result;
        count++;
        if(count==k) return pRoot;
        return KthNode(pRoot->right, k);
    }
};
```

- 迭代

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
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        stack<TreeNode*> stackTemp;
        TreeNode* pNode = pRoot;
        while(pNode || !stackTemp.empty()){
            if(pNode!=NULL){
                stackTemp.push(pNode);
                pNode = pNode->left;
            }else{
                pNode = stackTemp.top();
                stackTemp.pop();
                k--;
                if(k==0) return pNode;
                pNode = pNode->right;
            }
        }
        return NULL;
    }
};
```




