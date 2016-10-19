## NO.75. Sort Colors
 Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

 Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

 Note:
 You are not suppose to use the library's sort function for this problem.

 click to show follow up.

 Follow up:
 A rather straight forward solution is a two-pass algorithm using counting sort.
 First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.

 Could you come up with an one-pass algorithm using only constant space?

## 思路
1. Two-pass
第一次将0全部置换到最左边
第二次将1全部置换到中间

2. One-pass
将所有的0放在最左边，将所有的2放在最右边。
注意边界条件，当i == 0时只需要检查是否为2, 当i == 2时只需要检查是否为0。注意必须先置换2,再置换0。否则当置换2完毕之后，可能0没有及时的置换到左边，考虑如下序列[1, 2, 0]。

## 实现
```
#two-pass
class Solution {
public:
    void sortColorNum(vector<int>& nums, int& first, int num)
    {
            while(nums[first] == num && first < nums.size()) ++first;
            int second = nums.size() - 1;
            while(first < nums.size() && second > first)
            {
                while(nums[second] != num && second > first) --second;
                if(second == first) break;
                swap(nums[first], nums[second]);
                while(nums[first] == num && first < nums.size()) ++first;
            }
    }
    void sortColors(vector<int>& nums) {
        int first = 0;
        sortColorNum(nums, first, 0);
        sortColorNum(nums, first, 1);
    }
};

#one-pass
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int zero = 0, second = nums.size() - 1;
        for(int i = 0; i <= second; ++i)
        {
            while(i < second && nums[i] == 2) swap(nums[i], nums[second--]);
            while(i > zero && nums[i] == 0) swap(nums[i], nums[zero++]);
        }
    }
};
```
