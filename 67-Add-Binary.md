## NO.67. Add Binary

Given two binary strings, return their sum (also a binary string).

For example,
a = "11"
b = "1"
Return "100". 

## 思路
使用两个指针分别对对应的位逐位相加，并记住进位。
需要考虑的情况：
	1. 两个字符串长度不一样
	2. 最后完成运算后，进位不为零，需要将进位加到最后结果中
	3. 注意最终结果为‘0’的情况，在返回结果取子字符串时得保证至少有一个字符返回。

## 实现
```
class Solution {
public:
    string addBinary(string a, string b) {
        int sizeA = a.size(), sizeB = b.size();
        string ret(sizeA + sizeB + 1, '0');
        int indexA = sizeA - 1, indexB = sizeB - 1, indexRet = ret.size() - 1;
        int extra = 0;
        while(indexA >= 0 && indexB >= 0)
        {
            int sum = (a[indexA] == '1')
                        + (b[indexB] == '1') + extra;
            ret[indexRet] = sum % 2 + '0';
            extra = sum / 2;
            indexA--;
            indexB--;
            indexRet--;
        }
        while(indexA >= 0) 
        {
            int sum = (a[indexA--] == '1') + extra;
            ret[indexRet--] = sum % 2 + '0';
            extra = sum / 2;
        }
        while(indexB >= 0) 
        {
            int sum = (b[indexB--] == '1') + extra;
            ret[indexRet--] = sum % 2 + '0';
            extra = sum / 2;
        }
        if(extra) ret[indexRet] = extra + '0';
        indexRet = 0;
        while(ret[indexRet] == '0' && indexRet != ret.size() - 1) indexRet++;
        return ret.substr(indexRet);
    }
};
```