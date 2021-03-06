# 26.二叉搜索树与双向链表


## 26.二叉搜索树与双向链表

### 题目描述  

- 输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

&nbsp;


### 解题思路  

- 下面的思路来自《剑指offer》

将二叉搜索树转换成双向链表即当前结点的左指针指向左子树的最大结点，右指针指向右子树的最小节点，如下图所示：   

![](https://raw.githubusercontent.com/PhillipHuang2017/markdownimg/master/picgo/20191209211812.png)

因此，只需要使用中序遍历的顺序进行转换（中序遍历一棵二叉搜索树即是从小到大输出），然后用一个指针`pLastNode`指向已经转换的链表的最大结点（尾结点）即可，刚开始时这个指针为`NULL`。由于使用递归，当遍历到当前结点时，左子树已经完成了转换，因此，只需让`pLastNode`的右指针指向当前结点，当前结点的左指针指向`pLastNode`，让`pLastNode`指针指向当前结点，然后递归右子树即可。

最后在主函数里面需要查找到链表的头结点然后**返回头结点**。

那个`pLastNode`指针在整个过程中用来记录当前已完成转换的链表的未结点，作用相当于一个全局变量，因此可以用全局变量来做，或者也可以在主函数中创建这个`pLastNode`指针变量，然后将这个指针变量的指针传入调用的函数进行修改。如果不用上面两种方法的话，当前的`pLastNode`就只能不断地传入传出，增加了很多开销，这个在写代码的时候自然就能体会到。**最好的办法还是用全局变量，减少参数传递开销**。

&nbsp;

### 代码 

- 方法一，新手版本，`pLastNode`不断传入传出，产生很多不必要的开销

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
    TreeNode* ConvertNode(TreeNode* pNode, TreeNode* pLastNode){
        if(pNode->left)
            pLastNode = ConvertNode(pNode->left, pLastNode);
        
        pNode->left = pLastNode;
        if(pLastNode) pLastNode->right = pNode;
        pLastNode = pNode;
        
        if(pNode->right)
            pLastNode = ConvertNode(pNode->right, pLastNode);
        return pLastNode;
    }
public:
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        if(pRootOfTree==NULL) return NULL;
        ConvertNode(pRootOfTree, NULL);
        while(pRootOfTree->left)
            pRootOfTree = pRootOfTree->left;
        
        return pRootOfTree;
    }
};
```

- 方法二，使用指向指针的指针修改主函数中局部变量`pLastNode`的值，仅减少传出开销

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
    void ConvertNode(TreeNode* pNode, TreeNode** pLastNode){
        if(pNode->left)
            ConvertNode(pNode->left, pLastNode);
        
        pNode->left = *pLastNode;
        if(*pLastNode) (*pLastNode)->right = pNode;
        *pLastNode = pNode;
        
        if(pNode->right)
            ConvertNode(pNode->right, pLastNode);
    }
public:
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        if(pRootOfTree==NULL) return NULL;
        TreeNode* pLastNode = NULL;
        ConvertNode(pRootOfTree, &pLastNode);
        while(pRootOfTree->left)
            pRootOfTree = pRootOfTree->left;
        
        return pRootOfTree;
    }
};
```

- 方法三，使用全局变量`pLastNode`，同时减少传入和传出开销

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
    TreeNode* pLastNode = NULL;
    void ConvertNode(TreeNode* pNode){
        if(pNode->left)
            ConvertNode(pNode->left);
        
        pNode->left = pLastNode;
        if(pLastNode) pLastNode->right = pNode;
        pLastNode = pNode;
        
        if(pNode->right)
            ConvertNode(pNode->right);
    }
public:
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        if(pRootOfTree==NULL) return NULL;
        ConvertNode(pRootOfTree);
        while(pRootOfTree->left)
            pRootOfTree = pRootOfTree->left;
        
        return pRootOfTree;
    }
};
```


