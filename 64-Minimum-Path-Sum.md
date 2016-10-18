## NO.64. Minimum Path Sum
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

- 思路
每次计算到达gird[i][j]处的最小和是计算gird[i-1][j]和gird[i][j-1]的较小值。对于i == 0或者j == 0需要特殊考虑，只需要加上前一个值就可以。

- 实现
```
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        if(m == 0) return 0;
        int n = grid[0].size();
        if(n == 0) return 0;
        for(int i = 0; i < m; ++i)
            for(int j = 0; j < n; ++j)
            {
                if(!i && !j) continue;
                else if(!i && j) grid[i][j] += grid[i][j - 1];
                else if(i && !j) grid[i][j] += grid[i - 1][j];
                else grid[i][j] += min(grid[i - 1][j], grid[i][j - 1]);
            }
        return grid[m - 1][n - 1];
    }
};
```