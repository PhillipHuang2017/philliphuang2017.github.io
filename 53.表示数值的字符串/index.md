# 53.表示数值的字符串


## 53.表示数值的字符串

### 题目描述  

- 请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串`"+100","5e2","-123","3.1416"`和`"-1E-16"`都表示数值。 但是`"12e","1a3.14","1.2.3","+-5"`和`"12e+4.3"`都不是。

&nbsp;

### 解题思路  

- 要注意一些特殊情况：`.123, -.1, .1e2, 1.e2, 1.e-2, 1.`，这些都是合法的。
- 思路一，分类讨论。总共有数字、大小写的`'e'`、小数点，正负号这些字符，需要满足以下条件：
  - 正负号只允许出现在开头，或者紧跟`'e'`的后面；
  - `'e'`不能在开头或者结尾，且最多只能出现一次；
  - 小数点最多只能出现一次，且不能出现在指数部分（即`'e'`后面）；
  - 除上述字符和数字以外其他字符不允许出现。
- 思路二，正则表达式，`r'^[\+-]?\d*\.?\d*([eE][\+-]?\d+)?$'`
  - `[]`表示匹配其中的一个，`[mn]`表示匹配`m`或者`n`
  - `?`表示匹配0个或1个
  - `()`表示分组
  - 加号和小数点记得要转义

&nbsp;

### 代码 

- 思路一，分类讨论，C++。

```c
class Solution {
public:
    bool isNumeric(char* string)
    {
        if(string==NULL || string[0]=='\0') return false;
        
        int eCount = 0, dotCount = 0;
        for(int p=0; string[p]!='\0'; p++){
            if(string[p]=='+' || string[p]=='-'){
                // 正负号只允许出现在开头或者紧跟`'e'`的后面；
                if((p!=0 && string[p-1]!='e' && string[p-1]!='E') || string[p+1]=='\0')  
                    return false;
            }
            else if(string[p]=='.'){
                // 只能出现1次（因为指数不能是小数），且不能出现在指数部分（即e后面）
                // 小数点可以出现在开头和结尾，前面可以是正负号，后面可以是e
                if(dotCount>0 || eCount>0) return false;
                dotCount++;
            }
            else if(string[p]=='e' || string[p]=='E'){
                // e不能在开头或者结尾，且只能出现一次
                if(eCount>0 || p==0 || string[p+1]=='\0') 
                    return false;
                eCount++;
            }
            // 如果不是上面的字符的话只能是数字
            else if(string[p]<'0' || string[p]>'9') return false;
        }
        return true;
    }

};
```

- 思路二，正则表达式

  - Python2

    python3.4的话可以把`^`和`$`去掉，使用`re.fullmatch()`函数。

  ```python
  import re
  # -*- coding:utf-8 -*-
  class Solution:
      # s字符串
      def isNumeric(self, s):
          # write code here
          if re.match(r'^[\+-]?\d*\.?\d*([eE][\+-]?\d+)?$', s):
              return True
          else:
              return False
  ```

  

  - Java

  ```java
  //正则表达式解法
  public class Solution {
      public boolean isNumeric(char[] str) {
          String string = String.valueOf(str);
          return string.matches("[\\+\\-]?\\d*(\\.\\d+)?([eE][\\+\\-]?\\d+)?");
      }
  }
  /*
  以下对正则进行解释:
  [\\+\\-]?            -> 正或负符号出现与否
  \\d*                 -> 整数部分是否出现，如-.34 或 +3.34均符合
  (\\.\\d+)?           -> 如果出现小数点，那么小数点后面必须有数字；
                          否则一起不出现
  ([eE][\\+\\-]?\\d+)? -> 如果存在指数部分，那么e或E肯定出现，+或-可以不出现，
                          紧接着必须跟着整数；或者整个部分都不出现
  */
  ```




