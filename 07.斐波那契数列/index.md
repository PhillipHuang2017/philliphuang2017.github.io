# 07.斐波那契数列


## 7.斐波那契数列  

### 题目描述  
- 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。
- n<=39 


### 解题思路
很简单，不必多说，不要用递归就好，因为用递归的话重复计算次数太多，堆栈太多，既浪费空间又浪费时间。 



### C++代码
- 迭代（推荐）
```c
class Solution {
public:
    int Fibonacci(int n) {
        if (n<0){
            throw "n must >= 0";
        }
        else if(n == 0){
            return 0;
        }
        int a = 0;
        int b = 1;
        int temp = 0;
        int i = 1;
        while(i<n){
            temp = b;
            b = a + b;
            a = temp;
            i++;
        }
        return b;
        
    }
};
```

- 迭代的骚操作
```c
class Solution {
public:
    int Fibonacci(int n) {
        if (n<0){
            throw "n must >= 0";
        }
        else if(n == 0){
            return 0;
        }
        int a = 0, b = 1;
        while(--n){
            b += a;
            a = b-a;
        }
        return b;
        
    }
};
```

- 递归（强烈不推荐！效率极低，无法通过！）
```c
class Solution {
public:
    int Fibonacci(int n) {
        if (n<0){
            throw "n must >= 0";
        }
        else if(n == 0){
            return 0;
        }
        else if(n == 1){
            return 1;
        }
        else{
            return Fibonacci(n-2) + Fibonacci(n-1);
        }
        
    }
};
```
