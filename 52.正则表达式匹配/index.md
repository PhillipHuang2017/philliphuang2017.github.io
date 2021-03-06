# 52.正则表达式匹配


## 52.正则表达式匹配

### 题目描述  

- 请实现一个函数用来匹配包括`'.'`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但是与`"aa.a"`和`"ab*a"`均不匹配

&nbsp;

### 解题思路  

- 因为正则表达式后面一个字符为`'*'`的话有多种可能，因此需要用递归，要把各种特殊情况都考虑到，文字难以说清楚，见代码和注释，也是根据牛客网练习模式测试失败的案例不断调整最后才成功的。   


&nbsp;

### 代码 

- C++

```c
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        // 非法输入，不能出现开头就是'*'或者两个'*'连着的情况
        if(str==NULL || pattern==NULL || pattern[0]=='*') return false;
        // 同时结束，匹配成功
        if(str[0]=='\0' && pattern[0]=='\0') return true;
        // str没结束但pattern结束了，匹配失败
        // 注意：pattern没结束但str结束了不一定失败，比如str='\0', pattern='a*'
        // 但是在后面的处理中要注意防止str数组越界
        if(str[0]!='\0' && pattern[0]=='\0') return false;
        
        // 两个都没结束
        // 只能匹配一个字符
        if(pattern[1] != '*'){
            // 匹配成功，注意'.'不能匹配空字符串
            if(str[0]==pattern[0] || (pattern[0]=='.' && str[0]!='\0')) 
                return match(str+1, pattern+1);
            // 匹配失败
            else return false;
        }
        // 匹配0个或多个，即pattern[1]=='*'
        else{
            // 当前字符匹配成功，可能是0个也可能是多个，考虑'aaa'或'a'匹配'a*a*a*'的特殊情况
            // 注意'.'不能匹配空字符串
            if(str[0]==pattern[0] || (pattern[0]=='.' && str[0]!='\0')){
                // 匹配0个 || 1个或多个
                return match(str, pattern+2) || match(str+1, pattern);
            }
            // 当前字符匹配失败，即匹配0个
            else return match(str, pattern+2);
        }
    }
};
```

