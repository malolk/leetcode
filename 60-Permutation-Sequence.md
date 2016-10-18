## NO.60 Permutation Sequence

- Problem
The set [1,2,3,…,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):

    "123"
    "132"
    "213"
    "231"
    "312"
    "321"

Given n and k, return the kth permutation sequence.

Note: Given n will be between 1 and 9 inclusive.

- 思路
	1. 第一步初始化整个序列是升序排列。
	2. 从第一位开始计算后续位数所有的排列数，也就是除去第一位后的阶乘，将k减去1并除以这个阶乘得到的数就是结果中要获得的下一个数，如果除得结果为0,则表示当前数为结果中的下一位数，为什么需要将k减去1？因为k==1×M!则表示刚好后面的数的一次置换即为k，因此结果中的下一位数仍是第一位数。接着将k-1所除结果加入返回字符串并将其从字符数组中删除，这样字符数组中的字符还是升序排列，将k取k对M!的模。
	3. 如果k仍大于零则继续第二步，否则置换结束，另外如果vec中还有元素，应该将其转换成降序排列。因为当k==0时，表示刚好当前位置之后的一次全部置换可以等于k，因此需将后续序列变为降序获得后续序列的最后一个排列。

- 实现
```
class Solution {
public:
    string getPermutation(int n, int k) {
        vector<char> vec;
        for(int i = 1; i <= n; ++i) vec.push_back('0' + i);
        vector<int> multi(10, 1);
        for(int i = 1; i < 10; ++i)
            multi[i] = i*multi[i - 1];
        string str;
        while(k > 0)
        {
            int incr = 0;
            int kinds = multi[vec.size() - 1];
			// k - 1 for k == kinds
            incr = (k - 1) / kinds;
            k %= kinds;
            str.push_back(vec[incr]);
            vec.erase(vec.begin() + incr);
        }
        str.insert(str.end(), vec.rbegin(), vec.rend());
        return str;
    }
};
```
