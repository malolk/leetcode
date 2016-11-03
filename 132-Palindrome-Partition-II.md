## NO.132. Palindrome Partitioning II

Given a string s, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

For example, given s = "aab",
Return 1 since the palindrome partitioning ["aa","b"] could be produced using 1 cut.

## 思路
第一种思路：
首先将各个子字符串是否为回文的标记全部记录下来，接下来使用递归，每次递归时分割当前起始位置开始的一个回文子字符串，然后递归获得余下的字符串分割后的最小子字符串数，并将其加1得到以当前子字符串分割时的最小字符串数，然后换以当前起始位置开始的另一个更长的回文子字符串，进行上述相同的操作。每次递归时都记录分割后最小的子字符串数。最后将子字符串数减1得到最小的分割数。

第二种思路：
对上述的递归思路使用迭代的方式实现，首先计算以起始位置靠右的子字符串的最小分割数，然后将起始位置左移，则可使用靠右已经计算的结果来计算当前的最小分割数。

第三种思路：
只使用一个vector存储以每个节点结束的字符串的最小分割数。比如map[i]存储的是子字符串s[0...i-1]的最小分割数目，该vector的大小是size+1。接着使用字符串中的每个位置为中心，当回文字符串长度是奇数时，将左右长度从零增长到与当前位置相同的大小，分别计算左右两侧是否对称，计算的过程中可以利用前面中心点计算的结果，例如当前左边的长度是j，则有`map[i + j + 1] = min(map[i + j + 1], map[i - j])` 如果对称则判断是否分割数目是更小的，更小的则将原先的值更新。当回文字符串长度是偶数时，左右长度则需要从1开始算，在更新时会有所不同，`map[i + j + 1] = min(map[i + j + 1], map[i - j + 1])`。最后返回`map[size]`即可。

## 实现
```
class Solution {
public:    
    int minCut(string s) {
        if(s.size() == 0) return 0;
        vector<int> map(s.size(), -1);
        vector<vector<int>> dp(s.size(), vector<int>(s.size(), 0));
        for(int i = s.size() - 1; i >= 0; --i)
        {
            map[i] = s.size();
            for(int j = i; j < s.size(); ++j)
            {
                if(s[i] == s[j] && (j - i < 2 || dp[i + 1][j - 1]))
                {
                    dp[i][j] = 1;
                    if(j == s.size() - 1) map[i] = 0;
                    else map[i] = min(map[i], 1 + map[j + 1]);
                }
            }
        }
        return map[0];
    }   
};

class Solution {
public:    
    int minCut(string s) {
        if(s.size() == 0) return 0;
        vector<int> map(s.size() + 1, s.size());
        map[0] = -1;
        for(int i = 0; i < s.size(); ++i)
        {
            for(int j = 0; i - j >= 0 && i + j < s.size() && s[i - j] == s[i + j]; ++j)
                map[i + j + 1] = min(map[i + j + 1], 1 + map[i - j]);
            for(int j = 1; i - j + 1 >= 0 && i + j < s.size() && s[i - j + 1] == s[i + j]; ++j)
                map[i + j + 1] = min(map[i + j + 1], 1 + map[i - j + 1]);
        }
        return map[s.size()];
    }
};

class Solution {
public:    
    int minCut(string s) {
        if(s.size() == 0) return 0;
        vector<int> map(s.size() + 1, s.size());
        map[0] = -1;
        for(int i = 0; i < s.size(); ++i)
        {
            for(int j = 0; i - j >= 0 && i + j < s.size() && s[i - j] == s[i + j]; ++j)
                map[i + j + 1] = min(map[i + j + 1], 1 + map[i - j]);
            for(int j = 1; i - j + 1 >= 0 && i + j < s.size() && s[i - j + 1] == s[i + j]; ++j)
                map[i + j + 1] = min(map[i + j + 1], 1 + map[i - j + 1]);
        }
        return map[s.size()];
    }
};
```

