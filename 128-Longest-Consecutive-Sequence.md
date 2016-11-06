## NO.128 Longest Consecutive Sequence

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

For example,
Given [100, 4, 200, 1, 3, 2],
The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.

Your algorithm should run in O(n) complexity.

## 思路
使用set存储输入数据，相当于是去重，然后从set中的起始元素开始，以该元素为中心，分别寻找更大的连续元素和更小的连续元素，每找到一个连续元素则将计数加1,并将其从set中删除，接着继续寻找连续元素，如果左右两边都找不到连续的元素，说明此连续序列已经结束，将此序列的长度与结果进行比较，如果比结果值大则将结果值更新为此序列的长度。如果此时set仍然不为空，则接着以set中的第一个元素开始进行上述相同的操作。

## 实现
```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> unique(nums.begin(), nums.end());
        int max = 0;
        int cnt = 0;
        while(!unique.empty())
        {
            auto it = unique.begin();
            int num = *it;
            cnt = 1;
            unique.erase(it);
            if(num > INT_MIN)
            {
                int small = num - 1;
                // for smaller
                while(!unique.empty() && small >= INT_MIN)
                {
                    auto itSmall = unique.find(small);
                    if(itSmall == unique.end()) break;
                    unique.erase(itSmall);
                    cnt++;
                    if(small == INT_MIN) break;
                    small--;
                }
            }
            if(num < INT_MAX)
            {
                int big = num + 1;
                // for bigger
                while(!unique.empty() && big <= INT_MAX)
                {
                    auto itBig = unique.find(big);
                    if(itBig == unique.end()) break;
                    unique.erase(itBig);
                    cnt++;
                    if(big == INT_MAX) break;
                    big++;
                }
            }
            max = (max < cnt) ? cnt : max;
        }
        return max;
    }
};
```