# 17.树的子结构


## 17.树的子结构  

### 题目描述  

- 输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们**约定空树不是任意一个树的子结构**）    

### 解题思路  

- 首先要搞清楚**子树**和**子结构**的区别：

  - **子树**：如果B是A的子树，那么从A的某一个结点开始，一直到叶子结点要全都和B的结点相同（一直到根节点）；  
  - **子结构**（本题）：只要B是A里面的任意一个部分，都可以算作A的子结构，不像子树要求那么严格。  

- 判断子树时，那么在向下递归遍历的时候，遍历到最后需要两个树同时访问到空指针才算相同；     

- 判断子结构时，在向下遍历的时候，需要小树先遍历到空指针才算相同。  

- 主函数可以先判断B是否是从A的根结点开始的子结构或子树，不是的话再用A的左右结点进行递归。

- 题目**约定空树不是任意一个树的子结构**。  

  


### 代码 

- **子结构**（本题），题目中函数名为`HasSubtree`，但我感觉函数名应该改成`HasSubstruct`更合适，不然有歧义，因此这里用`HasSubstruct`。

```c
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
    bool HasSubstruct(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        if(pRoot2==NULL || pRoot1==NULL) return false;
        
        if(IsSubstruct(pRoot1, pRoot2)) return true;  //这一句其实可以并入下一句
        
        return HasSubstruct(pRoot1->left, pRoot2) || HasSubstruct(pRoot1->right, pRoot2);
    }
    
    bool IsSubstruct(TreeNode *pRoot1, TreeNode *pRoot2){
        if(pRoot2==NULL) return true;
        else if(pRoot1==NULL) return false;
        else if(pRoot1->val != pRoot2->val) return false; //这一句其实可以并入下一句
        else return IsSubstruct(pRoot1->left, pRoot2->left) && IsSubstruct(pRoot1->right, pRoot2->right);
    }
};
```

- **子树**（其实就只是第二个函数的规则稍微改一下）   

```c
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
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        if(pRoot2==NULL || pRoot1==NULL) return false;
        
        if(IsSubtree(pRoot1, pRoot2)) return true;  //这一句其实可以并入下一句
        
        return HasSubtree(pRoot1->left, pRoot2) || HasSubtree(pRoot1->right, pRoot2);
    }
    
    bool IsSubtree(TreeNode *pRoot1, TreeNode *pRoot2){
        if(pRoot2==NULL && pRoot1==NULL) return true;
        else if(pRoot1==NULL || pRoot2==NULL) return false;
        else if(pRoot1->val != pRoot2->val) return false; //这一句其实可以并入下一句
        else return IsSubtree(pRoot1->left, pRoot2->left) && IsSubtree(pRoot1->right, pRoot2->right);
    }
};
```








