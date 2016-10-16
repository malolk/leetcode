## NO.47 Permutations II
- Problem
Given a collection of numbers that might contain duplicates, return all possible unique permutations.

For example,[1,1,2] have the following unique permutations:
```
	[
		[1,1,2],
		[1,2,1],
		[2,1,1]
	]
```

- 思路
首先将序列排序为升序序列，即为“最小序列”，使用next permutation不断的产生更大的序列，直到序列变为降序序列，即为最大序列。
next permutation的思路：
第一步，从后往前找到第一个逆序对，即nums[i - 1]\<nums\[i\]。如果i==0, 则表示序列已经变为最大序列，因此返回false。
第二步，由于在i及之后的序列是降序序列，因此后续序列中可能有大于nums[i-1]的数，从i开始往后查找，找到最后一个大于nums[i-1]的最小的数，并将其与nums[i-1]进行交换。
第三步，将i及之后的降序序列排序为升序序列。

- 实现
```
class Solution {
public:
    bool nextPermute(vector<int>& nums)
    {
        int i = nums.size() - 1, k = -1;
        while(i > 0 && nums[i] <= nums[i - 1]) i--;
        if(i == 0) return false;
        
        k = i;
        while((k + 1) < nums.size() && nums[k + 1] > nums[i - 1])
            k++;
        swap(nums[i - 1], nums[k]);
        sort(nums.begin() + i, nums.end());
        return true;
    }
    
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> ret;
        sort(nums.begin(), nums.end());
        ret.push_back(nums);
        while(nextPermute(nums))
            ret.push_back(nums);
        return ret;
    }
};
``` 
