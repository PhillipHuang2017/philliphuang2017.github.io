# 55.链表中环的入口结点


## 55.链表中环的入口结点

### 题目描述  

- 给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

&nbsp;

### 解题思路  

- 思路一，保存所有访问过的结点的地址，某个地址第二次出现的时候该结点就是环的入口结点，时间复杂度$O(n^{2})$，空间复杂度$O(n)$。

- 思路二，快慢指针，来自[牛客网题解](https://www.nowcoder.com/profile/7141476/codeBookDetail?submissionId=12902117)。设置一个快指针（每次前进2步），一个慢指针（每次前进1步），如果有环的话，快指针一定会在环中和慢指针相遇，即追上慢指针（结论一，后面证明）；当两个指针相遇后，一个指针从链表开头开始走，另一个从相遇点开始走，每次前进1步，最终相遇的结点就是环入口结点（结论二，后面证明）。 

  **结论一证明**：   

  首先快指针一定比慢指针先入环，觉得可能不会相遇无非是考虑到会不会在环中每次快指针都刚好跳过慢指针，但是如果快指针下一步跳过慢指针的话，前提是这一次快指针和慢指针已经在同一个位置；如果这一次快指针在慢指针前一个位置的话，下一次就会相遇，如果快指针和慢指针在同一个位置的话这一次就已经相遇了，因此快指针一定会追上慢指针，而且当慢指针进入环中以后，快指针最晚也会在下一圈把慢指针追上（最慢的情况，慢指针进环的时候快指针刚好在下一个结点）。

  **结论二证明**==（记得以后把下面的图片传到自己的图床）==：   

  ![](https://uploadfiles.nowcoder.com/images/20180615/4240377_1529033184336_9A253E69EDBB4FD57BB16FC3A32C2756)

  假设表头到环入口的距离是`a`，环入口到相遇点距离为`b`，相遇点到环入口距离为`c`，那么结论一中相遇的时候快指针走过的路程为`a+(b+c)*k+b`，其中`k`为快指针走过的圈数且一定`k>=1`，慢指针走过的路程为`a+b`，另外还有一个隐含条件是快指针的路程为慢指针的2倍，因此得到`a+(b+c)*k+b=2(a+b)`，化简后得`a=(k-1)(b+c)+c`，从该公式可以看出，如果一个指针从相遇点开始走，另一个指针从表头开始走，一定会在环入口相遇。   


&nbsp;

### 代码 

- 思路二，快慢指针，C++

```c
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
        if(pHead==NULL || pHead->next==NULL) return NULL;
        
        ListNode *pFast, *pSlow;
        pFast = pSlow = pHead;
        while(pFast && pFast->next){
            pFast = pFast->next->next;
            pSlow = pSlow->next;
            if(pFast==pSlow) break;
        }
        if(pFast != pSlow) return NULL;
        
        pFast = pHead;
        while(pFast != pSlow){
            pFast = pFast->next;
            pSlow = pSlow->next;
        }
        return pFast;
    }
};
```




