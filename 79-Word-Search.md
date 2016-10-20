## NO.79. Word Search
Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,
Given board =

[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

word = "ABCCED", -> returns true,
word = "SEE", -> returns true,
word = "ABCB", -> returns false.

## 思路
使用递归，对每一种可能进行检查直到找到字符串，或者所有可能性找完则表示不存在。
**为防止用过的字符被使用一次以上，将使用的字符先mark为0, 等后续递归完成后再将其还原。**

## 实现
```
class Solution {
public:
    int lines = 0;
    int cols = 0;
    bool existImpl(vector<vector<char>>& board, 
                    int start, 
                    string& word, 
                    int index)
    {
        int line = start / cols;
        int col = start % cols;
        if(board[line][col] == word[index]) 
        {
            if(++index == word.size()) return true;
            char mark = board[line][col];
            board[line][col] = 0;
            for(int i = 0; i < 4; ++i)
            {
                if(i == 0 && col > 0)
                {   if(existImpl(board, start - 1, word, index)) return true;     }
                else if(i == 1 && col < cols - 1)
                {   if(existImpl(board, start + 1, word, index)) return true;     }
                else if(i == 2 && line > 0 )
                {   if(existImpl(board, start - cols, word, index)) return true;  }
                else if(i == 3 && line < lines - 1 )
                {   if(existImpl(board, start + cols, word, index)) return true;  }
            }
            board[line][col] = mark;
        }
        return false;
    }
    
    bool exist(vector<vector<char>>& board, string word)
    {
        if(word.empty()) return false;
        if((lines = board.size()) == 0) return false;
        else if((cols = board[0].size()) == 0) return false;
        for(int l = 0; l < lines; ++l)
            for(int c = 0; c < cols; ++c)
                if(existImpl(board, l*cols + c, word, 0)) return true;
        return false;
    }
};
```


