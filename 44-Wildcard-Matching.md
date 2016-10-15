## NO.44 Wildcard Matching

- Problem
Implement wildcard pattern matching with support for '?' and '*'.
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```
The matching should cover the entire input string (not partial).
The function prototype should be:
bool isMatch(const char *s, const char *p)
Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```

- 思路
整个算法的思路是使用回溯法。
使用两个index， indexS和indexP分别指向待匹配字符串和模式字符串，indexS遍历完待匹配字符串且indexP要么等于模式字符串的长度要么indexP之后（包括indexP）的位置全为'*',则表示匹配成功。
	1. 首先判断indexS处与indexP所指的值是否相等，或者indexP处的字符为‘？’，如果成立则将两个index加1,进入下一次遍历。否则判断当前indexP所指字符是否为'*', 如果不是则进入步骤2, 否则进入步骤3.
	2. 当前indexP所指字符不为'*', 查看logP(最近一次出现'*'的位置)是否初始化过，如果没有出现过'*'则一定不匹配。如果logP初始化过，则将indexP置为logP+1, 即重新从最近一次'*'后一个位置开始匹配，同时将logS（最近一次'*'替代的子字符串最后一个字符后一个位置）加1,则表示最近一次'*'匹配的子字符串增长1, 然后进入下一个循环。
	3. 当前indexP所指字符为'*'，因此将logP指向此'*'的位置， 并将logS设置当前没有匹配成功的字符，此时相当于'*'匹配的是空字符串。
	4. 最后待匹配的字符串遍历完毕后，检查余下的模式字符是否为空或者余下的模式字符全是'*'， 则表示匹配成功。

- 实现
```
class Solution {
public:
    bool isMatch(string s, string p) {
        int sLen = s.size(), pLen = p.size();
        int indexS = 0, indexP = 0;
        int logS = -1, logP = -1;
        while(indexS < sLen)
        {
            if(s[indexS] == p[indexP] || p[indexP] == '?')
            {
                indexS++;
                indexP++;
            }
            else if(p[indexP] != '*')
            {
                if(logP == -1)
                    return false;
                else
                {
                    indexS = ++logS;
                    indexP = logP + 1;
                }
            }
            else
            {
                logP = indexP++;
                logS = indexS;
            }
        }
        while(indexP < pLen && p[indexP] == '*')
            ++indexP;
        return indexP == pLen;
    }
};
```