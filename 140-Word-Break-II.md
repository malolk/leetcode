## NO.140. Word Break II
Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

For example, given
s = "catsanddog",
dict = ["cat", "cats", "and", "sand", "dog"].

A solution is ["cats and dog", "cat sand dog"].

## 思路
基本思想与Word Break II相近，在每次匹配完成时将字符串存入返回vector。另外使用一个vector来记录那些不匹配的字符串的起始位置。

## 实现
```
class Solution {
public:
    string joinStr(vector<string>& vec)
    {
        string ret;
        for(auto item : vec) ret.append(item);
        return ret;
    }
    bool impl(string& s, int start, vector<string>& buf, vector<string>& ret, unordered_set<string>& wordDict, vector<int>& map)
    {
        if(map[start] == -1) return false;
        bool flag = false, isValide = false;
        int size = s.size();
        for(int i = start; i < size; ++i)
        {
            string str = s.substr(start, i - start + 1);
            if(wordDict.count(str))
            {
                buf.push_back(str + " ");
                if(i == size - 1) 
                {
                    buf.back().resize(str.size());
                    ret.push_back(joinStr(buf));
                    isValide = true;
                }
                else isValide = impl(s, i + 1, buf, ret, wordDict, map);
                buf.pop_back();
                if(isValide)
                    if(!flag) flag = true;
            }
        }
        map[start] = flag ? 1 : -1;
        return flag;
    }
    vector<string> wordBreak(string s, unordered_set<string>& wordDict) {
        vector<string> buf;
        vector<string> ret;
        vector<int> map(s.size(), 0);
        impl(s, 0, buf, ret, wordDict, map);
        return ret;
    }
};
```
