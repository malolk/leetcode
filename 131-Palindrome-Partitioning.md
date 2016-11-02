## NO.131 Palindrome Partitioning

Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

For example, given s = "aab",
Return
```
[
  ["aa","b"],
  ["a","a","b"]
]
```

## 思路
使用回溯的方法，首先确定给定字符串的第一个回文字符串，然后使用递归对后续字符串进行相同的操作，当字符串起始位置为字符串大小时则将前缀字符串存入返回结果中。

## 实现
```
class Solution {
public:
    bool check(string s)
    {
        int i = 0, j = s.size() - 1;
        while(i < j)
            if(s[i++] != s[j--]) 
                return false;
        return true;
    }
    void impl(string& s, int start, vector<string>& buf, vector<vector<string>>& ret)
    {
        if(s.size() == start)
        {
            if(start) ret.push_back(buf);
            return;
        }
        for(int i = start; i < s.size(); ++i)
        {   
            string str = s.substr(start, i - start + 1); 
            if(check(str))
            {
                buf.push_back(str);
                impl(s, i + 1, buf, ret);   
                buf.pop_back();
            }
        }   
    }

    vector<vector<string>> partition(string s)
    {
        vector<vector<string>> ret;
        vector<string> buf;
        impl(s, 0, buf, ret);
        return ret; 
    }
};
```
