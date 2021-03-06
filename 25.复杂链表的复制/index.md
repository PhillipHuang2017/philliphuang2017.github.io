# 25.复杂链表的复制


## 25.复杂链表的复制

### 题目描述  

- 输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

&nbsp;


### 解题思路  

- 下面的思路来自《剑指offer》，使用分治法，将整个任务分成几个步骤完成，复杂问题简单化。

总共分为三步，每个步骤可以单独封装成一个函数完成：

1. 遍历链表，复制每个结点，如复制结点`A`得到`A1`，将结点`A1`插到结点`A`后面；
2. 重新遍历链表，复制老结点的随机指针给新结点，如`A1.random = A.random.next`;
3. 拆分链表，将链表拆分为原链表和复制后的链表。

**详细流程及图示**：

1. 遍历链表，复制每个结点，如复制结点`A`得到`A1`，将结点`A1`插到结点`A`后面；

![](https://raw.githubusercontent.com/PhillipHuang2017/markdownimg/master/picgo/20191209193636.png)

2. 重新遍历链表，复制老结点的随机指针给新结点，如`A1.random = A.random.next`;

![](https://raw.githubusercontent.com/PhillipHuang2017/markdownimg/master/picgo/20191209193712.png)

3. 拆分链表，将链表拆分为原链表和复制后的链表。

![](https://raw.githubusercontent.com/PhillipHuang2017/markdownimg/master/picgo/20191209193746.png)

&nbsp;

### 代码 

- 递归，简单，思路清晰

```c++
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
private:
    void CloneNodes(RandomListNode* pHead){
        if(pHead==NULL) return;
        RandomListNode* pTemp = new RandomListNode(pHead->label);
        pTemp->next = pHead->next;
        pHead->next = pTemp;
        CloneNodes(pTemp->next);
    }
    
    void ConnectRandomNode(RandomListNode* pHead){
        if(pHead==NULL) return;
        if(pHead->random){
            pHead->next->random = pHead->random->next;
        }
        ConnectRandomNode(pHead->next->next);
    }
    
    RandomListNode* SplitList(RandomListNode* pHead){
        if(pHead==NULL) return NULL;
        RandomListNode* pNewHead = pHead->next;
        pHead->next = pNewHead->next;
        if(pNewHead->next){
            pNewHead->next = pNewHead->next->next;
        }
        SplitList(pHead->next);
        return pNewHead;
    }

public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        CloneNodes(pHead);
        ConnectRandomNode(pHead);
        return SplitList(pHead);
    }
};
```

- 迭代

```c++
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
private:
    void CloneNodes(RandomListNode* pHead){
        while(pHead){
            RandomListNode* pTemp = new RandomListNode(pHead->label);
            pTemp->next = pHead->next;
            pHead->next = pTemp;
            pHead = pTemp->next;
        }
    }
    
    void ConnectRandomNode(RandomListNode* pHead){
        while(pHead){
            if(pHead->random){
                pHead->next->random = pHead->random->next;
            }
            pHead = pHead->next->next;
        }
    }
    
    RandomListNode* SplitList(RandomListNode* pHead){
        if(pHead==NULL) return NULL;
        RandomListNode* pNewHead = pHead->next;
        while(pHead){
            RandomListNode* pNewListNode = pHead->next;
            pHead->next = pNewListNode->next;
            if(pNewListNode->next){
                pNewListNode->next = pNewListNode->next->next;
            }
            pHead = pHead->next;
        }
        return pNewHead;
    }

public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        CloneNodes(pHead);
        ConnectRandomNode(pHead);
        return SplitList(pHead);
    }
};
```


