# 19.顺时针打印矩阵


## 19.顺时针打印矩阵

### 题目描述  

- 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字）。

- 例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

- （实际代码通过返回一个按打印顺序排列的数组来实现）

  ​    

### 解题思路  

- 思路一，先计算要打印多少圈，然后每次记录下来当前剩下的矩阵的`left`，`right`，`top`，`bottom`，在大循环里面每次打印一圈，当某一圈结束后`left`大于`right`或者`top`大于`bottom`就结束循环，顶点统一归到行里面（不统一也行，但是要清楚每个顶点归到哪一次循环了），但是要注意最后只剩一行或者一列或者一个数字的时候别重复了，因此四个循环中只有第一个循环是每次一定会执行的。具体看代码，比较好理解。**推荐使用这个方法**。

- 思路二，通过每次改变方向后打印的次数规律，每打印一行，下一次再按行的方向打印的时候，打印次数就会少一个，下标移动的方向也会反过来，列也是一样的。因此在while循环中包含两个循环，一个按行打印，一个按列方向打印，每次打印完都把下次按该方向打印的次数减一，将下标方向反转。

  - 但是这样有个问题，就是不好判断什么时候结束，因此先计算出总共要打印多少个数字，没打印一个数字，该变量就减一，该变量减到0就结束。

  - 这个方法不是很好，代码也不太好写，思路也麻烦，**不建议这个方法**，很垃圾。




### 代码 

- 思路一（推荐）

```c
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> result;
        int row_len = matrix.size();
        int column_len = matrix[0].size();
        if(row_len==0 || column_len==0) return result;
        
        int left=0, right=column_len-1, top=0, bottom=row_len-1;
        while(left<=right&&top<=bottom){
            for(int i=left;i<=right;i++)
                result.push_back(matrix[top][i]);
            if(top<bottom-1)
                for(int i=top+1;i<=bottom-1;i++)
                    result.push_back(matrix[i][right]);
            if(left<=right&&top<bottom)
                for(int i=right;i>=left;i--)
                    result.push_back(matrix[bottom][i]);
            if(top<bottom-1&&left<right)
                for(int i=bottom-1;i>=top+1;i--)
                    result.push_back(matrix[i][left]);
            left++;right--;top++;bottom--;
        }
        return result;
    }
};
```

- 思路二（垃圾）

```c
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> result;
        int row_len = matrix.size();
        int column_len = matrix[0].size();
        if(row_len==0 || column_len==0) return result;
        
        int column_step = 1, row_step = 1;
        int column = -1, row = 0;
        int num = row_len*column_len;
        int steps;
        while(num){
            if(column_len>0){
                steps = column_len;
                while(steps--){
                    column += column_step;
                    result.push_back(matrix[row][column]);
                    num--;
                }
                column_step = -column_step;
                column_len--;
            }
            
            if(row_len>0){
                row_len--;
                steps = row_len;
                while(steps--){
                    row += row_step;
                    result.push_back(matrix[row][column]);
                    num--;
                }
                row_step = -row_step;
            }
        }
        return result;
    }
};
```


