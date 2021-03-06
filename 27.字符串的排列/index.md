# 27.字符串的排列


## 27.字符串的排列

### 题目描述  

- 输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。  

  输入描述:

  > 输入一个字符串,长度不超过9(可能有字符重复)，字符只包括大小写字母。

&nbsp;

### 解题思路  

下面的思路来自《剑指offer》。

- 可以把问题转换为，第一个字符固定，求后面的字符串的所有排列，使用递归的方式解决；

- 思想如下面的图所示（结合代码看更容易理解）：

  ![](https://raw.githubusercontent.com/PhillipHuang2017/markdownimg/master/picgo/20191212200827.png)

- 首先分别将每个字符放在首位，即将每个字符和第一个字符交换，得到上面的树的第二层，然后第二层中对于每个结果，将第一个字符固定，依次将后面的字符放在第二位，得到第三层，这时候只剩下最后一个了，没得换了，所有的组合就确定了。

- 这里要注意，递归时传字符串的时候要传值，而不能是指针或者引用，这样每次递归的时候拿到的就是不同的字符串。

- 但是可能存在重复的情况，比如输入`"aba"`，`"aa"`，的时候结果就会产生重复。因此放入结果数组中的时候要先检查是否已经在结果中了，可以使用标准库中的`find`函数。

  ```c++
  template <class InIt, class T>
  InIt find(InIt first, InIt last, const T& val);
  //其功能可以是在迭代器 first、last 指定的容器的一个区间 [first, last) 中，按顺序查找和 val 相等的元素。如果找到，就返回该元素的迭代器；如果找不到，就返回 last。
  ```

- 为了方便判题程序判题，要求结果以字典序输出，字典序就是按字母的顺序，因此最终结果还要再排一下序，调用标准库的`sort`函数即可。

  ```c++
  template<class_RandIt>
  void sort(_RandIt first, _RandIt last);
  //该算法可以用来对区间 [first, last) 从小到大进行排序。
  ```

  

&nbsp;

### 代码 

```c++
class Solution {
private:
    void Search(string str, vector<string>& result, int begin){
        if(begin == str.size()-1){
            if(find(result.begin(), result.end(), str) == result.end())
                result.push_back(str);
            return;
        }
        for(int i=begin; i<str.size(); i++){
            char temp = str[begin];
            str[begin] = str[i];
            str[i] = temp;
            
            Search(str, result, begin+1);
            
            //这里要先换回来再进行下一次循环
            str[i] = str[begin];
            str[begin] = temp;
        }
        
    }
    
public:
    vector<string> Permutation(string str) {
        vector<string> result;
        if(str.empty()) return result;
        Search(str, result, 0);
        sort(result.begin(), result.end());
        return result;
    }
};
```

