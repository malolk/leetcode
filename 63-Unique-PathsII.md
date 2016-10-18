## NO.63. Unique Paths II 

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

For example,

There is one obstacle in the middle of a 3x3 grid as illustrated below.

[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]

The total number of unique paths is 2.

Note: m and n will be at most 100.

- 思路

首先算出到达每个位置的种数，每个位置的种数取决于来自left和above的种数，从左上角到右下角依次复制，对于有绊脚石的位置则将其置为0。值得注意的地方是第一行和第一列，因此需要单独考虑。

- 实现

```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        if(m == 0) return 0;
        int n = obstacleGrid[0].size();
        for(int i = 0; i < m; ++i)
            for(int j = 0; j < n; ++j)
            {
                if(obstacleGrid[i][j] == 1) 
                    obstacleGrid[i][j] = 0;
                else if(i == 0 || j == 0) 
                {
                    if(i == 0 && j == 0) obstacleGrid[i][j] = 1;
                    else if(i == 0 && j != 0) obstacleGrid[i][j] = obstacleGrid[i][j - 1];
                    else if(i != 0 && j == 0) obstacleGrid[i][j] = obstacleGrid[i - 1][j];
                }
                else
                {
                    int left = obstacleGrid[i - 1][j];
                    int above = obstacleGrid[i][j - 1];
                    obstacleGrid[i][j] = left + above;
                }
            }
        return obstacleGrid[m - 1][n - 1];
    }
};
```