## NO.91. Decode Ways
A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26

Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2.

## 思路
使用递归，每个循环分别解析一个或者两个字符，返回为两种情况的和。

## 实现
```
class Solution {
public:
    int impl(string s, int start, vector<int>& map)
    {
        int oneMatch = 0, twoMatch = 0;
        char c1 = s[start];
        if(start == s.size() || c1 == '0') return 0;
        else if(map[start] != -1) return map[start];
        oneMatch = (start + 1 == s.size()) ? 1 : impl(s, start + 1, map);
        if((start + 1) < s.size())
        {
            char c = s[start + 1];
            bool check = c >= '0' && ((c1 == '1' && c <= '9') ||
                            (c1 == '2' && c <= '6'));
            if(check) twoMatch = (start + 2 == s.size()) ? 1 : impl(s, start + 2, map);
        }
        map[start] = oneMatch + twoMatch;
        return oneMatch + twoMatch;
    }
    int numDecodings(string s) {
        vector<int> map(s.size(), -1);
        return impl(s, 0, map);
    }
};

```

```
class Solution {
public:
    int numDecodings(string s) {
        int size = s.size();
        vector<int> map(size + 1, 0);
        if(size == 0) return 0;
        if(size >= 1) map[size - 1] = (s[size - 1] == '0') ? 0 : 1;
        for(int i = size - 2; i >= 0; --i)
        {
            if(s[i] != '0')
            {
                int tmp = stoi(s.substr(i, 2));
                if(map[i + 1] > 0) map[i] += map[i + 1];
                if(tmp >= 1 && tmp <= 26)
                {
                    if(map[i + 2] > 0) map[i] += map[i + 2];
                    else if((i + 2) == size) map[i] += 1;
                }
            }
        }
        return map[0];
    }
};

```