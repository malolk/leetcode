## NO.89 Maximal Rectangle

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

For example, given the following matrix:
```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```
Return 6.

## 思路
每次只计算矩阵的0～i行，然后将包括这些行的矩阵比作一个直方图，每一列的高度得根据最底层行的字符来判断，如果最下面的字符为‘0’则高度为0,否则其高度为行数i-1时的高度加上1,计算出直方图的高度后即可使用stack的方法计算最大矩形的面积。最后统计出以每行为底时的最大矩形面积。

## 实现

```
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int m = matrix.size();
        if(m == 0) return 0;
        int n = matrix[0].size();
        if(n == 0) return 0;
    
        vector<int> buf(m + 1, 0); 
        stack<int> stk;
        int ret = 0;
        for(int len = 1; len <= n; ++len)
        {
            for(int i = 0; i < m; )
            {
                int temp = matrix[i][len - 1] == '1' ? (buf[i] + 1) : 0;
                if(stk.empty() || buf[stk.top()] < temp)
                {
                    buf[i] = temp;
                    stk.push(i++);
                }
                else
                {
                    int height = buf[stk.top()];
                    stk.pop();
                    int leftIndex = stk.empty() ? 0 : (stk.top() + 1); 
                    ret = max(ret, height*(i - leftIndex));
                }
            }
            while(!stk.empty())
            {
                int height = buf[stk.top()];
                stk.pop();
                int leftIndex = stk.empty() ? 0 : (stk.top() + 1);
                ret = max(ret, height * (m - leftIndex));
            }
        }
        return ret;    
    }   
};
```
