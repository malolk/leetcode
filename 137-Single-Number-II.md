## NO.137 Single Number II
Given an array of integers, every element appears three times except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

## 思路
这种题目有一个一般的解法。假设除了其中一个数，其他的数都重复了M次。
那么可以构造一个以M为基的计数器，每个bit若是被加了M的整数倍次则将会归零。
以此题为例M为3, 则构造一个状态转移机制分别记录每个bit的状态，每次当bit位累加次数为3的倍数时则自动清零。设置a b记录bit c的累加次数。则可得到如下推导：
```
		// construct a base three counter
        // state bits a b, incoming bits c
        // ab = 00, c = 0/1, <ab> = 00/01
        // ab = 01, c = 0/1, <ab> = 01/10
        // ab = 10, c = 0/1, <ab> = 10/00
        // a = a & ~b & ~c | ~a & b & c
        // b = ~a & ~b & c | ~a & b & ~c
```
在实现时是数字而不是比特，但是使用位操作运算可以将数中的每一位按照上述的公式进行处理。最后得到的值是a和b的或运算，因为只要最终结果与a和b对应的bit上出现过1就可以，无论是1次还是2次，通过或运算即可知道最终结果对应比特上是否为1。

## 实现
```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        // construct a base three counter
        // state bits a b, incoming bits c
        // ab = 00, c = 0/1, <ab> = 00/01
        // ab = 01, c = 0/1, <ab> = 01/10
        // ab = 10, c = 0/1, <ab> = 10/00
        // a = a & ~b & ~c | ~a & b & c
        // b = ~a & ~b & c | ~a & b & ~c
        int a = 0, b = 0, num = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            num = nums[i];
            int temp = a & ~b & ~num | ~a & b & num;
            b = ~a & ~b & num | ~a & b & ~num;
            a = temp;
        }
        return a | b;
    }
};
```
