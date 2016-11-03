## NO.115 Distinct Subsequences

Given a string S and a string T, count the number of distinct subsequences of T in S.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).

Here is an example:
S = "rabbbit", T = "rabbit"

Return 3.

## 思路
使用动态规划进行解题，此题目可以归结为计算两个字符串距离的问题。假设c(0...i - 1)转换为t(0...j - 1)有map[i][j]中转换方法。其中获得map[i][j]的方式分为两种，当`c[i-1]==t[j-1]`时，`map[i][j] = map[i - 1][j] + map[i - 1][j - 1]`, 当不相等时，`map[i][j] = map[i - 1][j]`。

## 实现
```
class Solution {
public:
    int numDistinct(string c, string t)
    {
        int m = c.size(), n = t.size();
        if(!m && n) return 0;
        int map[n + 1] = { 1, 0};
        int last = 1;
        for(int i = 1; i <= m; ++i)
        {
            last = 1;
            for(int j = 1; j <= n; ++j)
            {
                int temp = map[j];
                if(c[i - 1] == t[j - 1])
                    map[j] += last;
                last = temp;
            }
        }
        return map[n];
    }
};
```