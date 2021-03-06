# 61.序列化二叉树


## 61.序列化二叉树

### 题目描述  

- 请实现两个函数，分别用来序列化和反序列化二叉树

  二叉树的序列化是指：把一棵二叉树按照某种遍历方式的结果以某种格式保存为字符串，从而使得内存中建立起来的二叉树可以持久保存。序列化可以基于先序、中序、后序、层序的二叉树遍历方式来进行修改，序列化的结果是一个字符串，序列化时通过 某种符号表示空节点（#），以 ！ 表示一个结点值的结束（value!）。

  二叉树的反序列化是指：根据某种遍历顺序得到的序列化字符串结果str，重构二叉树。

&nbsp;

### 解题思路  

- 思路一，序列化后的数字人看不懂。根据牛客网的题解和牛客OJ系统的测试来看，只需要将二叉树序列化就行，不需要序列化后的字符串人能看懂（但题目的意思应该是要能看懂才行），而且C++里面不能很方便的将数字转换成字符串或者将字符串转换成数字。因此可以直接按前序遍历（深度优先搜索）的顺序把数字依次存储到`int`数组里面，如果某个结点为`NULL`则存入`0x80000001`，即最大的负数，因为这个数字出现的概率比较低。然后返回的时候把数组头指针由`int`指针转换成`char*`即可，虽然这样输出的结果人看不懂（因为`char`为1字节，`int`为4字节），但是可以反序列化回去，反序列化的时候只要把`char*`重新转换成`int*`即可。

  由于不知道需要多大的`int`数组，因此可以先用`vector`数组，将二叉树转换成数组以后获取数组的长度，然后把数字依次复制到`int`数组中。

- 思路二，按题目要求，序列化后的结果人能看懂，但是用C++的话会比较麻烦，暂时先把牛客网题解上别人的Java代码记录在这里，以后有空在写C++的。这个如果纯C语言的话就比较麻烦，因为要把`int`转换成`char`很麻烦，如果是C++的话有`string`类型，可以用`to_string(val)`函数把数字转换成字符串，用`stoi(s,p,b)`等函数将字符串转换成数字，该函数表示把字符串`s`从`p`开始转换成`b`进制的`int`，用`substr(pos,n)`方法从整个字符串中截取数字，用`c_str()`方法将字符串转换成字符数组。


&nbsp;

### 代码 

- 思路一，序列化后的数字人看不懂。

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
    // 深度优先搜索（前序遍历），这个参数buf声明为全局变量也可以，减少参数传递次数
    void dfs1(TreeNode* pNode, vector<int> &buf){
        if(pNode==NULL) {
            buf.push_back(0x80000001);
            return;
        }
        buf.push_back(pNode->val);
        dfs1(pNode->left, buf);
        dfs1(pNode->right, buf);
    }
    
    // 注意这个参数p必须是全局变量或者引用，而已思考一下为什么
    TreeNode* dfs2(int* &p){
        if(*p==0x80000001) {
            p++;
            return NULL;
        }
        TreeNode* pNode = new TreeNode(*p);
        p++;
        pNode->left = dfs2(p);
        pNode->right = dfs2(p);
        return pNode;
    }
public:
    char* Serialize(TreeNode *root) {    
        if(root==NULL) return NULL;
        vector<int> buf;
        dfs1(root, buf);
        int bufSize = buf.size();
        int* result = new int[bufSize];
        for(int i=0; i<bufSize; i++) result[i] = buf[i];
        return (char*)result;
    }
    TreeNode* Deserialize(char *str) {
        if(str==NULL) return NULL;
        int* p = (int*)str;
        return dfs2(p);  // 这里不能直接传入(int*)str，因为临时变量不能作为非const的引用
    }
};
```

- 思路二，按题目要求，序列化后的结果人能看懂，Java。

```java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;
 
    public TreeNode(int val) {
        this.val = val;
 
    }
 
}
*/
/*
    算法思想：根据前序遍历规则完成序列化与反序列化。所谓序列化指的是遍历二叉树为字符串；所谓反序列化指的是依据字符串重新构造成二叉树。
    依据前序遍历序列来序列化二叉树，因为前序遍历序列是从根结点开始的。当在遍历二叉树时碰到Null指针时，这些Null指针被序列化为一个特殊的字符“#”。
    另外，结点之间的数值用逗号隔开。
*/
public class Solution {
    int index = -1;   //计数变量
  
    String Serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        if(root == null){
            sb.append("#,");
            return sb.toString();
        }
        sb.append(root.val + ",");
        sb.append(Serialize(root.left));
        sb.append(Serialize(root.right));
        return sb.toString();
  }
    TreeNode Deserialize(String str) {
        index++;
        //int len = str.length();
        //if(index >= len){
        //    return null;
       // }
        String[] strr = str.split(",");
        TreeNode node = null;
        if(!strr[index].equals("#")){
            node = new TreeNode(Integer.valueOf(strr[index]));
            node.left = Deserialize(str);
            node.right = Deserialize(str);
        }
        return node;
  }
}
```




