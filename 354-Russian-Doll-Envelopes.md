## NO.354 Russian Doll Envelopes

## Problems
You have a number of envelopes with widths and heights given as a pair of integers (w, h). One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.

What is the maximum number of envelopes can you Russian doll? (put one inside other)

Example:
Given envelopes = [[5,4],[6,4],[6,7],[2,3]], the maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]). 

## 思路
这个题目与求解最长增长子序列那题很像，但是这题使用的是pair，即两个元素，因此有一些不同的地方。**通过解此题获得的经验是尽量将多维的问题化解为一维的问题，并将其转化为已知的问题**。
主要有两个步骤：
1. 将输入的数进行排序，以宽进行升序排列，如果宽相等则按照高降序排列。
2. 在重新排序好的前提下，对高组成的序列求取最长增长子序列。
这里将宽进行升序排列符合题意，而宽相等时按照高降序排列的目的是在求解增长子序列时防止将宽相等的两个envelopes都当作是有效的，使用降序排列则只会选择宽度相等高度较小的那个。

## 实现
```
class Solution {
public:
    static bool comparator(const pair<int, int>& lhs, const pair<int, int>& rhs) {
        if (lhs.first == rhs.first) {
            return (lhs.second > rhs.second);
        } else {
            return (lhs.first < rhs.first);
        }
    }
    
    int maxEnvelopes(vector<pair<int, int>>& envelopes) {
        int size = envelopes.size();
        if (size == 0) return 0;
        sort(envelopes.begin(), envelopes.end(), comparator);
        vector<int> tails(size, -1);
        tails[0] = envelopes[0].second;
        int len = 1;
        for (int i = 1; i < size; ++i) {
            int cur = envelopes[i].second;
            if (cur > tails[len - 1]) {
                ++len;
                tails[len - 1] = cur;
            } else {
                int left = -1, right = len - 1;
                while ((right - left) > 1) {
                    int mid = left + (right - left) / 2;
                    if (cur > tails[mid]) {
                        left = mid;
                    } else {
                        right = mid;
                    }
                }
                tails[right] = cur;
            }
        }
        return len;
    }
};
```
