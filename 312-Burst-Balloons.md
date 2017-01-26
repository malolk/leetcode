## NO.312 Burst Balloons

## Problem
```
 Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

 Find the maximum coins you can collect by bursting the balloons wisely.

 Note:
 (1) You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.
 (2) 0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100

 Example:

 Given [3, 1, 5, 8]

 Return 167 
```
```
  nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
  coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

## 思路
怎么将这个问题分解为更小的问题是解决这个问题的关键。如果使用第一个踩破气球的位置将数组分为两半，这样并没有包括两边可能产生影响的情况。所以当时我想的是利用最后一次踩破的气球将数组分为两个不相互影响的左右两个子数组，但是如何分解我没有仔细想清楚，因为最后一个被踩破的气球是与左右子数组有联系的，如何将这个特性联系起来我没有想清楚。最后看到讨论区是确定每个子数组的边界，这样就可以满足上面的要求。我得到的启发是在进行分解的每一步必须进行一定的处理，从而总的计算量在每一步减少。

## 实现
由上面的思路可以很快得出相应的递归实现。
```
class Solution {
public:
    int impl(vector<vector<int>>& cache, vector<int>& nums, int left, int right) {
        if (left + 1 == right) return 0;
        if (cache[left][right] > 0) return cache[left][right];
        
        int ret = 0;
        for (int i = left + 1; i < right; ++i) {
            ret = max(ret, nums[left] * nums[i] * nums[right] 
                      + impl(cache, nums, left, i) + impl(cache, nums, i, right));
        }
        cache[left][right] = ret;
        return ret;
    }
    
    int maxCoins(vector<int>& nums) {
        int size = nums.size();
        if (size == 0) return 0;
        size += 2;
        vector<vector<int>> cache(size, vector<int>(size, 0));
        vector<int> nums_copy(size, 1);
        for (int i = 1; i < size - 1; ++i)
            nums_copy[i] = nums[i - 1];
        return impl(cache, nums_copy, 0, size - 1);
    }
};
```

非递归版本，实现思路是首先计算长度较小的子数组。
```
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int size = nums.size();
        if (size == 0) return 0;
        size += 2;
        vector<vector<int>> cache(size, vector<int>(size, 0));
        vector<int> nums_copy(size, 1);
        for (int i = 1; i < size - 1; ++i)
            nums_copy[i] = nums[i - 1];
        for (int len = 1; len <= (size - 2); ++len) {
            for (int left = 0; left <= (size - len - 2); ++left) {
                int right = left + len + 1;
                for (int i = left + 1; i < right; ++i) {
                    cache[left][right] = max(cache[left][right], nums_copy[left] * nums_copy[i] * nums_copy[right] 
                                             + cache[left][i] + cache[i][right]);
                }
            }
        } 
        return cache[0][size - 1];
    }
};
```
