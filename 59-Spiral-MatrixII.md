## NO.59. Spiral Matrix II

- Problem

	Given an integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.
	For example, Given n = 3, You should return the following matrix:
```
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

- 思路
转弯时改变x和y的范围。

- 实现
```
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        if(n == 0) return {};
        int size = n*n;
        vector<int> elem(n, 0);
        vector<vector<int>> ret(n, elem);
        int x_start = 0, y_start = 0;
        int x_limit = n - 1, y_limit = n - 1;
        int x = 0, y = 0;
        int index = 0;
        while(index < size)
        {
            if(x == x_start && y == y_start)
            {
                for(; x <= x_limit; ++x)
                    ret[y][x] = ++index;
                y_start++; x--; y++;
            }
            else if(x == x_limit && y == y_start)
            {
                for(; y <= y_limit; ++y)
                    ret[y][x] = ++index;
                x_limit--; x--; y--;
            }
            else if(x == x_limit && y == y_limit)
            {
                for(; x >= x_start; --x)
                    ret[y][x] = ++index;
                y_limit--; x++; y--;
            }
            else if(x == x_start && y == y_limit)
            {
                for(; y >= y_start; --y)
                    ret[y][x] = ++index;
                x_start++; x++; y++;
            }
        }
        return ret;
    }
};
```

