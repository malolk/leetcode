##NO.93. Restore IP Addresses
Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:
Given "25525511135",

return ["255.255.11.135", "255.255.111.35"]. (Order does not matter) 

## 思路
IP的限制：
	1. 每一个class的数字不能超过3位。
	2. 每一个class上如果是一位可以是0到9,如果是多位则第一位不能为0。
	3. 每一位的数字不能大于255
使用回溯法则可以解析可能的IP地址。

## 实现
```
class Solution {
public:
    vector<string> impl(string s, int c)
    {
        vector<string> buf;
        int size = s.size();
        if(!(size <= (4 - c)*3 && (size >= (4 - c)))) return buf;
        int times = (s[0] == '0' ? 1 : min(3, size));
        if(c == 3)
        {
            if(s[0] == '0' && size > 1 || stoi(s) > 255)
            {}
            else buf.push_back(s);
            return buf;
        }
        for(int i = 0; i < times; ++i)
        {
            string tmp = s.substr(0, i + 1) + '.';
            if(stoi(tmp) > 255) continue;
            vector<string> bufTmp = impl(s.substr(i + 1), c + 1);
            if(bufTmp.empty()) continue;
            for(auto item : bufTmp)
                buf.push_back(tmp + item);
        }
        return buf;
    }
    vector<string> restoreIpAddresses(string s) {
        return impl(s, 0);
    }
};
```