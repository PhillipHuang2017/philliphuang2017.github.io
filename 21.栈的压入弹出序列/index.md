# 21.栈的压入、弹出序列


## 21.栈的压入、弹出序列

### 题目描述  

- 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

&nbsp;


### 解题思路  

- 思路一，模拟入栈出栈，创建一个临时栈`temp_stack`，首先创建入栈序列下标变量`pin`和出栈序列下标变量`pout`，两个变量都是从各自序列的头部开始（也就是初始化为0），当`pin`指向的数字个`pout`指向的数字不相等的时候，把`pin`指向的数字入栈，`pin`向后移动，`pout`不动；如果相等的话这个数字就不入栈（相当于先入栈然后立刻出栈），`pin`和`pout`都向后移动。这样的话入栈序列一定是先遍历完毕的，入栈序列遍历完毕以后，开始将`temp_stack`中的数字和出栈序列中剩下的数字同步出栈，即`temp_stack`当前出栈的数字要和`pout`指向的数字相同，并且`temp_stack`每出栈一个数字，`pout`就向后移动，如果某次出栈的时候数字不一样的话就直接返回false。
- 思路二，用循环对入栈顺序进行遍历，每次循环的时候将入栈序列的数字入栈，然后while循环当`temp_stack`非空且栈顶与`pout`指向的数字相等的时候，就出栈，并且`pout`向后移动。这样，当循环结束的时候，如果栈非空则返回`false`。但我感觉这个方法**不如思路一好**，思路一有些数字可以不用入栈，而这个思路的话所有数字都要入栈，这个思路也不是很流畅，而且还有嵌套循环，这个思路只是代码稍微少一点而已。

&nbsp;

### 代码 

- 思路一（推荐）

```c
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        int size = pushV.size();
        if(size != popV.size()) return false;
        if(size == 0) return true;
        stack<int> temp_stack;
        int pin=0, pout=0;
        while(pin < size){
            if(pushV[pin] != popV[pout]) temp_stack.push(pushV[pin]);
            else pout++;
            pin++;
        }
        while(!temp_stack.empty()){
            if(temp_stack.top() == popV[pout]){
                temp_stack.pop();
                pout++;
            }
            else return false;
        }
        return true;
    }
};
```

- 思路二（不推荐）

```c
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        int size = pushV.size();
        if(size != popV.size()) return false;
        if(size == 0) return true;
        stack<int> temp_stack;
        int pout=0;
        for(int pin=0; pin<size; pin++){
            temp_stack.push(pushV[pin]);
            while(!temp_stack.empty() && temp_stack.top()==popV[pout]){
                temp_stack.pop();
                pout++;
            }
        }
        return temp_stack.empty();
    }
};
```

