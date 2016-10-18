## NO.54 Spiral Matrix

- Problem 
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

For example,
	Given the following matrix:

	[
		[ 1, 2, 3 ],
		[ 4, 5, 6 ],
		[ 7, 8, 9 ]
	]

You should return [1,2,3,6,9,8,7,4,5]. 

- 思路
每次转弯时都更改x和y的大小范围。

- 实现
```
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if(matrix.size() == 0 || matrix[0].size() == 0) return {};
        int y_limit = matrix.size() - 1, x_limit = matrix[0].size() - 1;
        int y_start = 0, x_start = 0;
        const int size = (y_limit + 1)*(x_limit + 1);
        int x = 0, y = 0;
        vector<int> ret(size, 0);
        int len = 0;
        while(len < size)
        {
            if(x == x_start && y == y_start)
            {
                for(; x <= x_limit; ++x)
                    ret[len++] = matrix[y][x];
                y_start++;
                x = x_limit; y = y_start;
            }
            else if(x == x_limit && y == y_start)
            {
                for(; y <= y_limit; ++y)
                    ret[len++] = matrix[y][x];
                x_limit--;
                x = x_limit; y = y_limit;
            }
            else if(x == x_limit && y == y_limit)
            {
                for(; x >= x_start; --x)
                    ret[len++] = matrix[y][x];
                y_limit--;
                x = x_start; y = y_limit;
            }
            else if(x == x_start && y == y_limit)
            {
                for(; y >= y_start; --y)
                    ret[len++] = matrix[y][x];
                x_start++;
                x = x_start; y = y_start;
            }
        }
        
        return ret;
    }
};
```
