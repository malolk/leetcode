## NO.130. Surrounded Regions

 Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

For example,

X X X X
X O O X
X X O X
X O X X

After running your function, the board should be:

X X X X
X X X X
X X X X
X O X X

## 思路
1. 首先找到边界上的‘O’点，然后使用DFS将与其和其相邻的所有
‘O’点都置为‘1’， 最后那些没有置为‘1’的‘O’点则置为‘X’，最后再将‘1’置为‘O’点即可。
2. 使用Union-Find算法，始终将处在边界的‘O’节点选为根节点，find方法可以使用路径压缩的方法加快find的执行速度。对整个board进行Union完后，再检查每个位置，如果是‘O’点且其根节点不为边界节点，则将其置为‘X’。

## 实现
```
class Solution {
public:
    int findOp(int i, int* indexes)
    {
        while(indexes[i] != i) indexes[i] = indexes[indexes[i]], i = indexes[i];
        return i;
    }
    void unionOp(int i, int j, int* indexes, int m, int n)
    {
        int grpI = findOp(i, indexes);
        int grpJ = findOp(j, indexes);
        if(grpI == grpJ) return;
        int lineI = grpI / n, colI = grpI % n;
        if(lineI == 0 || lineI == (m - 1) || colI == 0 || colI == (n - 1)) 
            indexes[grpJ] = grpI;
        else
            indexes[grpI] = grpJ;
    }   
    void solve(vector<vector<char>>& board) {
        int m = board.size();
        if(m == 0) return;
        int n = board[0].size();
        if(n == 0) return;
        int size = m * n;
        int indexes[size]  = { 0 };
        for(int i = 0; i < size; i++)
        {
            indexes[i] = i;
            int line = i / n;
            int col = i % n;
            if(board[line][col] == 'X') continue;
            if(line != 0 && board[line - 1][col] == 'O')
                unionOp(i, i - n, indexes, m, n); 
            if(col != 0 && board[line][col - 1] == 'O')
                unionOp(i, i - 1, indexes, m, n); 
        }
        for(int i = 0; i < size; i++)
        {
            int line = i / n;
            int col = i % n;
            if(board[line][col] == 'X') continue;
            int root = findOp(i, indexes);
            int lineRoot = root / n, colRoot = root % n;
            if(lineRoot > 0 && lineRoot < (m - 1) && colRoot > 0 && colRoot < (n - 1))
                board[line][col] = 'X';
        }
    }
};
```