## NO.139 Word Break

Given a string s and a dictionary of words dict, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

For example, given
s = "leetcode",
dict = ["leet", "code"].

Return true because "leetcode" can be segmented as "leet code".

## 思路
1. 使用动态规划进行递归。每次取得字符串第一个在dict中的word，然后使用递归计算余下的字符串，如果余下的字符串是有效的则返回true，否则延长第一个word直到下一个较长的word在dict中找到，接着类似上述的方式对余下的字符串进行递归。如果找不到与dict中匹配的word，则返回true。
2. 使用动态规划迭代法，分配一个vector记录以每一个位置结束的字符串是否是有效的。每次计算下一个位置时，都会找到之前的有效字符串的结束位置，并判断与当前位置之间的字符串是否是有效的，如果无效，则继续寻找更早的有效字符串结束点，接着上述操作。直到判断以当前位置为结束点的字符串为有效的，或者前面所有的有效字符串结束点都已经考虑完毕则表示以当前点为字符串结束点的字符串不是有效字符。

## 实现
```
class Solution {
public:
    bool wordBreakImpl(string& s, int start, unordered_set<string>& wordDict, vector<int>& map) {
        if(map[start]) return map[start] > 0 ? true : false;
        int size = s.size();
        bool ret = false;
        
        for(int i = start; i < size; ++i)
        {
            if(wordDict.count(s.substr(start, i - start + 1)))
            {
                if(i == size - 1) ret = true;
                else if(wordDict.count(s.substr(i + 1))) ret = true;
                else ret = wordBreakImpl(s, i + 1, wordDict, map);
                if(ret) break;
            }
        }
        map[start] = ret ? 1 : -1;
        return ret;
    }
    bool wordBreak(string s, unordered_set<string>& wordDict)
    {
        vector<int> map(s.size(), 0);
        return wordBreakImpl(s, 0, wordDict, map);
    }
};
```

```
class Solution {
public:
    bool wordBreak(string s, unordered_set<string>& wordDict)
    {
        vector<bool> map(s.size() + 1, false);
        map[0] = true;
        for(int i = 1; i <= s.size(); ++i)
            for(int j = i - 1; j >= 0; --j)
            {
                if(map[j] && wordDict.count(s.substr(j, i - j)))
                {
                    map[i] = true;
                    break;
                }
            }
        return map[s.size()];
    }
};
```