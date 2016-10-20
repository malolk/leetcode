## NO.81. Search in Rotated Sorted Array II
Follow up for "Search in Rotated Sorted Array":
What if duplicates are allowed?

Would this affect the run-time complexity? How and why?

Write a function to determine if a given target is in the array.

## 思路
由于存在重复的数，如果最左边的值和最右边的值相同则会导致无法判断中点mid到底落在左边的升序序列中还是右边的升序序列中。因此需要将右边与最左边重复的数过滤掉使得整个右边的升序序列是小于左边的升序序列。
接下来使用常规的二分查找，但是注意在左边序列中找较小值时可能需要跳转到右边序列查找，在右边序列中找较大值时可能需要跳转到左边序列查找。

## 实现
```
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int size = nums.size();
        int left = 0, right = size - 1;
        while(right > left && nums[right] == nums[left]) --right;
        while(left < right)
        {
            int mid = (left + right)/2;
            if(target == nums[mid]) return true;
            else if(target < nums[mid])
            {
                if(nums[mid] <= nums[right]) right = mid; // right part
                else if(target >= nums[left] ) right = mid; // left part
                else if(target <= nums[right]) left = mid + 1; // move to right
                else return false;
            }
            else 
            {
                if(nums[mid] >= nums[left]) // left part
                    left = mid + 1;
                else if(target <= nums[right]) left = mid + 1; // right part
                else if(target >= nums[left]) right = mid; // move to left
                else return false;
            }
        }
        return (nums[right] == target);
    }
};
```
