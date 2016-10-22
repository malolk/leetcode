## NO.76. Minimum Window Substring
 Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

For example,
S = "ADOBECODEBANC"
T = "ABC"

Minimum window is "BANC".

Note:
If there is no such window in S that covers all characters in T, return the empty string "".

If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S. 

## 思路
使用两个数组table和table_checker，大小为256,分别存放目标字符的次数和待搜索的字符串中的字符的次数。使用两个指针分别指示匹配字符的起始和结束，另外使用一个bit-mask来只是是否已经匹配成功，只有当table_checker中的各个字符的个数大于等于table中相应字符的个数时才能说是已经匹配。步骤如下：
	1. 使用first向后移动直到所指位置字符是给定字符串中的字符。
	2. 如果second小于first说明这是第一次循环，则将其置为first，向后移动每次移动到有效字符则在table_checker中增加对应字符的次数，并检查是否大于table中相应字符的次数，如果是则将bit-mask中的对应位进行置位并检查是否完全匹配，如果匹配，则进入下一个步骤。
	3. 试图向后移动first直到恰好不能完全匹配，此过程中不断记录匹配范围的最小区间。
	4. 由于要进行下一个循环，而second所指位置已经检查过了，因此将其向后移动一位。

## 寻找满足条件的子字符串的模板
```
int findSubstring(string s){
        vector<int> map(128,0);
        int counter; // check whether the substring is valid
        int begin=0, end=0; //two pointers, one point to tail and one  head
        int d; //the length of substring
        for() { /* initialize the hash map here */ }
        while(end<s.size()){
            if(map[s[end++]]-- ?){  /* modify counter here */ }
            while(/* counter condition */){
                 /* update d here if finding minimum*/
                //increase begin to make it invalid/valid again
                if(map[s[begin++]]++ ?){ /*modify counter here*/ }
            }
            /* update d here if finding maximum*/
        }
        return d;
  }
```
## 实现
```
class Solution {
public:
    void set(unsigned char* checker, char c)
    { checker[c/8] |= (1 << (c % 8)); }
    void unset(unsigned char* checker, char c)
    { checker[c/8] &= ~(1 << (c % 8)); }
    bool isValid(unsigned char* checker, char c)
    { return checker[c/8] & (1 << (c % 8)); }
    bool isMatch(unsigned char* checker, unsigned char* full_checker)
    {
        for(int i = 0; i < 32; ++i)
            if(checker[i] ^ full_checker[i]) return false;
        return true;
    }
    string minWindow(string s, string t) {
        vector<int> table(256, 0);
        vector<int> table_checker(256, 0);
        unsigned char full_checker[32] = {0}, checker[32] = {0};
        for(int i = 0; i < t.size(); ++i)
        {
            table[t[i]]++;
            set(full_checker, t[i]);
        }
        int n = s.size();
        int start = 0, end = n - 1;
        int first = 0, second = -1;
        bool ready = false;
        while(first < n && second < n)
        {
            while(first < n)
            {
                if(isValid(full_checker, s[first])) break;
                ++first;
            }
            if(first == n) break;
            if(first > second) second = first ;
            while(second < n)
            {
                char c = s[second];
                if(isValid(full_checker, c) &&
                    ++table_checker[c] >= table[c])
                {
                      set(checker, c);
                      if(isMatch(checker, full_checker)) break;
                }
                ++second;
            }
            if(second == n) break;
            while(first <= second)
            {
                ready = true;
                if((second - first) < (end - start)) { start = first; end = second; }
                char c = s[first++];
                if(isValid(full_checker, c) && --table_checker[c] < table[c])
                {
                    unset(checker, c);
                    break;
                }
            }
            second++;
            if(first > second) second = first;
        }
        if(ready)
            return s.substr(start, end - start + 1);
        else return "";
    }
};

```
```
concise solution
class Solution {
public:

    string minWindow(string s, string t) {
        vector<int> table(128, 0);
        int counter = t.size();
        int n = s.size();
        int first = 0, second = 0;
        int start = 0, end = n + 1;
        for(auto item : t) table[item]++;
        while(second < n)
        {
            if(table[s[second++]]-- > 0) counter--;
            while(counter == 0)
            {
                if((second - first) < (end - start)) 
                {
                    start = first;
                    end = second;
                }
                if(table[s[first++]]++ == 0) counter++;
            }
        }
        if(end == (n + 1)) return "";
        else return s.substr(start, end - start);
    }
};

```