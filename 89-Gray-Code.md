## NO.89.Gray Code
The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

For example, given n = 2, return [0,1,3,2]. Its gray code sequence is:
```
00 - 0
01 - 1
11 - 3
10 - 2
```
Note:
For a given n, a gray code sequence is not uniquely defined.

For example, [0,2,3,1] is also a valid gray code sequence according to the above definition.

For now, the judge is able to judge based on one instance of gray code sequence. Sorry about that.

## 思路
1. 观察序列的变化，比特位数没增加一位，则会在以前的序列的前面加上一个1。只不过存储时是倒序存储。
........1 时，为0,1
........2 时，在为1时的前面加上1为0, 1, 11, 10
........3 时，在为2时的前面加上1为0, 1, 11, 10, 110, 111, 011, 010

2. 使用函数公式，考察函数f(i)=i^(i>>1), 那么f(i+1)与f(i)的关系是只有一个比特位不同。
```
f(i)^f(i+1) = i^(i>>1)^(i+1)^((i+1)>>1)
			= (i^(i+1))^((i^(i+1))>>1)
令g(i) = i ^ (i+1), g(i)得到的比特形式会是...0000111...11。
f(i)^f(i+1) = g(i)^(g(i)>>1), 此项的比特形式会是...0000100...00。
因此f(i)只与f(i+1)只有一位不相同。
另外当f(i)==f(j)时，只能是i==j, 假设i==j+n，则有
g(i) = i ^ (i+n)，可以知道g(i)肯定不为0,考虑g(i)不为零的最高位的那个比特，那么当f(i)^f(i+n) = g(i)^(g(i)>>1)时，不为零的最高位的那个比特上也不会为零，说明f(i)与f(j)不同。
```

## 实现
```
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> ret(1, 0);
        for(int i = 0; i < n; ++i)
        {
            int cnt = ret.size();
            while(cnt)
            {
                int curNum = ret[--cnt];
                curNum += (1 << i);
                ret.push_back(curNum);
            }
        }
        return ret;
    }
};

class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> ret(1 << n, 0);
        for(int i = 0; i < (1 << n); ++i)
            ret[i] = i^(i>>1);
        return ret;
    }
};
```


