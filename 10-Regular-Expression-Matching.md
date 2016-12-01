## NO.10.Regular Expression Matching

## Problem
Implement regular expression matching with support for `.` and `*`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```
## Solution

使用动态规划求解
维持一个dp数组，其中dp[i][j]表示p[0, j - 1]能否与s[0, i - 1]匹配。

- 初始化数组
当s和p都为空时表示匹配，dp[0][0]为true； 当p为空而s不为空，表示对于任意的i>=1，都会有dp[i][0]为false；当s为空而p不为空时，要使dp[0][j]为true，必须保证dp[0][j-2]为true且p[j - 1]为`*`。
- 递推关系
假如当前p[j - 1]不为`*`，必须得是当前p[j - 1]和s[i - 1]相等，或者p[j - 1]为字符'.'，此时有`dp[i][j] = dp[i - 1][j - 1]`;
假如当前p[j - 1]为`*`，则有两种情况：第一种情况是当前字符和前一个字符匹配零个字符，则有`dp[i][j] = dp[i][j - 2]`；第二种情况是当前字符和前一个字符匹配s[i]，则又分两种情况，字符`*`和前一个字符是否匹配了s[0, i-1]中的字符，如果没有匹配，则有`dp[i][j] = dp[i - 1][j - 2]`，如果之前有过匹配，则有`dp[i][j] = dp[i - 1][j]`, 实际上前一种情况是后一种情况的特殊情况，即字符'*'和前一个字符没有匹配s[0, i-1]中的字符。所以只需考虑`dp[i    ][j] = dp[i - 1][j - 2]`。
- 关于正则表达式
正则表达式的第一个字符不能为`*`, 另外在`*`出现时是将s中的字符与`*`的前一个字符进行匹配。

## 实现
```
class Solution {
public:
    bool isMatch(string s, string p) {
        int slen = s.size(), plen = p.size();
        if(plen == 0) return slen == 0;
        vector<vector<bool>> dp(slen + 1, vector<bool>(plen + 1, false));
        dp[0][0] = true;
        for(int i = 1; i <= slen; ++i) 
            dp[i][0] = false;
        for(int i = 1; i <= plen; ++i)
            // matching empty when p[i - 1] == '*' && dp[0][i - 2] == true
            dp[0][i] = i > 1 && p[i - 1] == '*' && dp[0][i - 2];
        for(int i = 1; i <= slen; ++i)
            for(int j = 1; j <= plen; ++j)
            {
                if(p[j - 1] != '*') 
                    dp[i][j] = (s[i - 1] == p[j - 1] || '.' == p[j - 1]) && dp[i - 1][j - 1];
                else 
                    // no need to account for j > 1, for the definition of regx
                    dp[i][j] = dp[i][j - 2] || ((s[i - 1] == p[j - 2] || '.' == p[j - 2]) && dp[i - 1][j]);
            }
        return dp[slen][plen];
    }
};
```

```
class Solution {
public:
    bool isMatch(string s, string p) {
        int slen = s.size(), plen = p.size();
        if(plen == 0) return slen == 0;
        if(plen > 1 && p[1] == '*')
            return isMatch(s, p.substr(2)) || slen && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p);
        else 
            return slen && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p.substr(1));
    }
};
```



