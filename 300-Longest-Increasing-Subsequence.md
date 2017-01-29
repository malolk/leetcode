## NO.300 Longest Increasing Subsequence

## Problem
 Given an unsorted array of integers, find the length of longest increasing subsequence.

 For example,
 Given [10, 9, 2, 5, 3, 7, 101, 18],
 The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

 Your algorithm should run in O(n2) complexity.

 Follow up: Could you improve it to O(n log n) time complexity? 

## 思路
思路一：
对于输入的数组nums，当每次计算以第i个元素作为结尾元素的增长序列的长度时，需要在i之前的序列上进行扩展，因此需要一个额外的数组记录这些不同结尾元素对应的序列的长度，并记住以不同的元素结尾的序列的最大长度，当处理完所有的元素时将最大长度返回即可。

```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int size = nums.size();
        if(!size) return 0;
        vector<int> lenArray(size, 1);
        int ret = 1;
        for (int i = 1; i < size; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    lenArray[i] = max(lenArray[i], lenArray[j] + 1);
                }
            }
            ret = max(ret, lenArray[i]);
        }
        return ret;
    }
};
```

思路二：
使用一个数组tails记录每个长度对应的结尾元素，同一个长度有多个递增序列时应该取结尾元素值较小的那个。只有在以下两种情况下更新tails数组。
1. 如果nums[i]的值大于所有不同长度的结尾元素，则将长度len递增，并在tails数组后存储新的长度的结尾元素。
2. 如果nums[i]的值在已有不同长度的结尾元素之间，如tails[m] < nums[i] <= tails[m + 1],　此时应该找到tails[m + 1]，将其更新。可以证明不同长度的结尾元素是递增的关系，因此可以使用二分查找，二分查找时由于需要查找大于等于的元素，这里二分查找使用的left为-1，将其作为左边的限制，由于在while循环中使用的是`(right - left) > 1`，因此不会出现mid和left与right相等的情况，当只有一个元素时，right所指的元素即为返回值。**但是如果将这个二分查找作为普通的查找算法时，需要首先检查数组中是否存在大于等于目标元素的值（比如数组中只有一个元素，目标元素大于数组中所有的元素）**。

```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int size = nums.size();
        if(!size) return 0;
        vector<int> tails(size, -1);
        tails[0] = nums[0];
        int len = 1;
        for (int i = 1; i < size; ++i) {
            if (nums[i] > tails[len - 1]) {
                ++len;
                tails[len - 1] = nums[i];
            } else {
                int left = -1, right = len - 1;
                while ((right - left) > 1) {
                    int mid = left + (right - left) / 2;
                    if (nums[i] > tails[mid]) {
                        left = mid;
                    } else {
                        right = mid;
                    }
                }
                tails[right] = nums[i];
            }
        }
        return len;
    }
};
```
