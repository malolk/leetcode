## NO.36. Valid Sudoku

Determine if a Sudoku is valid, according to: Sudoku Puzzles - The Rules.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

Note:
A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated. 

## 思路
首先检查每个小九宮格是否是有效的，再检查通过对角元素的横竖两条线上的元素是否是有效的。

## 实现
```
class Solution {
public:
    bool check(char c, int& checker)
    {
        if(c == '.') return true;
        else if(c < '1' || c > '9') return false;
        else if(checker & (1 << (c - '0'))) return false;
        else checker |= (1 << (c - '0'));
        return true;
    }
    bool isValidSudoku(vector<vector<char>>& board) {
        for(int i = 0; i < 9; ++i)
        {
            int x = (i % 3) * 3;
            int y = (i / 3) * 3;
            int checker = 0;
            for(int x_ = x; x_ < x + 3; ++x_)
                for(int y_ = y; y_ < y + 3; ++y_)
                    if(!check(board[y_][x_], checker))
                        return false;
        }
        for(int i = 0; i < 9; ++i)
        {
            int checker = 0;
            for(int x = 0; x < 9; ++x)
                if(!check(board[i][x], checker))
                    return false;
            checker = 0;
            for(int y = 0; y < 9; ++y)
                if(!check(board[y][i], checker))
                    return false;
        }
        return true;
    }
};
```