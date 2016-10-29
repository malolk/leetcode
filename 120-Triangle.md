## NO.120 Triangle

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

Note:
Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

## 思路
使用一个数组存放每一层的累加值，每次计算当前值时，**使用从后往前填充，这样在使用一个数组时就不会将接下来还要使用的上次计算的结果给覆盖掉**。

## 实现
```
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int row = triangle.size();
        if(row == 0) return 0;
        else if(row == 1) return triangle[0][0];
        vector<int> cur(row, 0);
        int ret = INT_MAX;
        cur[0] = triangle[0][0];
        for(int i = 2; i <= row; ++i)
        {
            for(int j = i - 1; j >= 0; --j)
            {
                if(j == i - 1) cur[j] = cur[j - 1] + triangle[i - 1][j];
                else if(j == 0) cur[j] += triangle[i - 1][j];
                else cur[j] = min(cur[j], cur[j - 1]) + triangle[i - 1][j];
                if(i == row) ret = min(ret, cur[j]);
            }
        }
        return ret;
    }
};
```