## NO.80.Remove Duplicates from Sorted Array II
Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

For example,
Given sorted array nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3. It doesn't matter what you leave beyond the new length. 

## 思路
1. 使用两个指针，第一个指针指示需要替换的位置，第二个指针用来查找下一个值。
2. 使用一个计数器i记录下一个应该存放的位置，另一个计数器j对nums进行遍历。只有满足i < 2 || nums[j] > nums[i - 2]时才会进行存储。这个方法比较巧妙，当i < 2时肯定可以存储，当i >= 2时，会有以下两种情况
	- nums[i - 2] == nums[i - 1]
	- nums[i - 2] < nums[i - 1]
这时，只要保证nums[j] > nums[i - 2]就可以存储。

## 实现
```
method 1
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int size = nums.size();
        if(!size && size <= 2) return size; 
        int first = 0, second = 0;
        int last = nums[0];
        if(nums[first + 1] == nums[first]) first++;
        first++;
        while(first < size)
        {
            if(nums[first] <= last)
            {
                second = first + 1;
                while(second < size && nums[second] <= last) ++second;
                if(second == size) break;
                swap(nums[first++], nums[second++]);
                while(second < size && nums[second] != nums[first - 1]) ++second;
                if(second != size)
                    swap(nums[first++], nums[second++]);
                last = nums[first - 1];
            }
            else
            {
                second = first + 1;
                while(second < size && nums[second] != nums[first]) ++second;
                if(second != size)
                    swap(nums[++first], nums[second]);
                last = nums[first++];
            }
        }
        return first;
    }
};

method 2
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int size = nums.size();
        int i = 0, j = 0;
        for(; j < size; ++j)
            if(i < 2 || nums[j] > nums[i - 2])
                nums[i++] = nums[j];
        return i;
    }
};
```