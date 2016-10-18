## NO.62. Unique Paths
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

- 思路
	1. 使用递归，当m == 1 || n == 1时表示只有一种路径到达目的地。
	2. 使用DP，计算出到达每一点的路径。
	3. 直接使用数学公式，但是会溢出，因此不好处理。

- 实现
```
class Solution {
public:
    int uniquePaths(int m, int n) {
        if(m == 1 || n == 1) return 1;
        else if(m == 0 || n == 0) return 0;
        return 2 * uniquePaths(m - 1, n - 1) +
                    uniquePaths(m - 2, n) +
                     uniquePaths(m, n - 2);
    }
};
```
```
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> matrix(m, vector<int>(n, 1));
        for(int i = 1; i < m; ++i)
            for(int j = 1; j < n; ++j)
                matrix[i][j] = matrix[i - 1][j] + matrix[i][j - 1]; 
        return matrix[m - 1][n - 1];
    }
};
```
```
class Solution {
public:
    int uniquePaths(int m, int n) {
        if(m > n) return uniquePaths(n, m);
        vector<int> cur(m, 1);
        vector<int> prev(m, 1);
        for(int j = 1; j < n; ++j)
        {
            for(int i = 1; i < m; ++i)
                cur[i] = cur[i - 1] + prev[i]; 
            swap(cur, prev);
        }
        return prev[m - 1];
    }
};
```