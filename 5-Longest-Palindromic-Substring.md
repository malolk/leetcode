# NO. 5 Longest Palindromic Substring

## 思路
September 12, 2016 2:57 PM

设当前获得的字串最长长度为curLen, 若以当前字符结尾的回文字符串长度要么为curLen+1, 要么为curLen+2。举例如下：
给定字符串： bebxxxxxa, 其中字符‘x’代表任意字符，当前字符为字符‘a’
* 若‘xxxa’为回文字符串， 则最大字符串长度增长为curLen+1;
* 若‘xxxxa’为回文字符串， 则最大字符串长度增长为curLen+2;
* 不需要检测以字符‘a’结尾且长度小于等于curLen的字符串;
* 也不需要检测长度大于curLen+2的字符串，因为若存在curLen+3的字符串为回文字符串，
则去掉头尾两个字符，剩下的curLen+1个字符仍为回文字符串，这与当前最长回文字符串为curLen矛盾。

## 实现代码
```
#include <string>
#include <iostream>
using namespace std;
bool isPalindrome(const string& s, int first, int last)
{
    while(first < last)
        if(s[first++] != s[last--])
            return false;
    return true;
}
string longestPalindrome(string s) {
    int size = s.size();
    if(size <= 1) return s;
    int currentLen = 0;
    string ret;
    for(int i = 0; i < size; ++i)
    {   
        if(isPalindrome(s, i - currentLen, i)) 
        {
            ret = s.substr(i - currentLen, currentLen + 1); 
            currentLen += 1;
        }
        else if(isPalindrome(s, i - currentLen - 1, i)) 
        {
            ret = s.substr(i - currentLen - 1, currentLen + 2); 
            currentLen += 2;
        }
    }   
    return ret;
}
int main()
{
    string str = "bb";
    cout << longestPalindrome(str) << endl;
    return 0;   
}

```
