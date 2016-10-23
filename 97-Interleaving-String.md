## NO.97. Interleaving String
Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

For example,
Given:
s1 = "aabcc",
s2 = "dbbca",

When s3 = "aadbbcbcac", return true.
When s3 = "aadbbbaccc", return false.

## 思路
将s3的第一个字符分别与s1和s2的第一个字符进行匹配，有三种情况：
	1. s3[0]只与s1[0]匹配，则将s1和s3都舍去第一个字符后，递归调用查看是否匹配。
	2. s3[0]只与s2[0]匹配，则将s2和s3都舍去第一个字符后，递归调用查看是否匹配。
	3. s3[0]与s1[0]和s2[0]都匹配，则分别执行步骤1和2中的操作。
为了避免不必要的调用，可以使用unordered_map记录已经判断的情况。

## 实现
```
class Solution {
public:
    bool isInterleaveImpl(string s1, string s2, string s3, unordered_map<string, bool>& map) {
        int size1 = s1.size(), size2 = s2.size(), size3 = s3.size();
        if(size1 == 0) return s2 == s3;
        else if(size2 == 0) return s1 == s3;
        else if((size1 + size2) != size3) return false;
        string key = s1 + to_string(size1) + s2 + to_string(size2) + s3;
        if(map.count(key)) return map[key];
        char check = s3[0];
        bool ret = (check == s1[0] && isInterleaveImpl(s1.substr(1), s2, s3.substr(1), map))
                    || (check == s2[0] && isInterleaveImpl(s1, s2.substr(1), s3.substr(1), map));
        map[key] = ret;
        return ret;
    }
    bool isInterleave(string s1, string s2, string s3) {
        unordered_map<string, bool> map;
        return isInterleaveImpl(s1, s2, s3, map);
    }
};
```