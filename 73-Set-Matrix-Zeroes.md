## NO.73. Set Matrix Zeroes
Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place. 

## 思路
将需要置为零的行和列分别存储在最左边的一列和最上面的一行，以零标记。同时使用topZero和leftZero标记最后是否需要将最上边的一行和最左边的一列置为零。在最后置零的过程中先根据之前的状态对i >= 1 && j >= 1的单元置零。最后根据topZero和leftZero标记判断是否对最左边的一列和最上面的一行进行置零。

## 实现

```
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        bool topZero = false, leftZero = false;
        int sizeM = matrix.size();
        if(sizeM == 0) return;
        int sizeN = matrix[0].size();
        if(sizeN == 0) return;
        for(int i = 0; i < sizeM; ++i)
        {
            for(int j = 0; j < sizeN; ++j)
            {
                if(!matrix[i][j])
                {
                    if(i == 0 && !topZero) topZero = true;
                    if(j == 0 && !leftZero) leftZero = true;
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for(int i = 1; i < sizeN; ++i)
            if(!matrix[0][i]) 
                for(int j = 0; j < sizeM; ++j)
                    matrix[j][i] = 0;
        for(int i = 1; i < sizeM; ++i)
            if(!matrix[i][0]) 
                for(int j = 0; j < sizeN; ++j)
                    matrix[i][j] = 0;
        if(topZero)
            for(int i = 0; i < sizeN; ++i)
                matrix[0][i] = 0;
        if(leftZero)
            for(int i = 0; i < sizeM; ++i)
                matrix[i][0] = 0;
    }
};
```