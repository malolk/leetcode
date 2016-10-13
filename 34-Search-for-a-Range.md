## NO.34 Search for a Range
- Problem
Given a sorted array of integers, find the starting and ending position of a given target value.
Your algorithm's runtime complexity must be in the order of O(log n).
If the target is not found in the array, return [-1, -1].
For example,
Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4]. 
- 思路
首先计算mid元素和target的关系，如果nums[target]<=nums[mid]，则表示要么还没有找到，应该将右边的right=\>mid, 或者已经找到，如果已经找到target，则后续nums[mid]将会小于等于target（因为是有序序列），只有在等于时将right=\>mid, 如果nums[mid]小于target，则表示mid之前的元素都不会包含target，因此将left=\>mid + 1。需要注意的地方是边界情况，假如有4个元素，那么left==0, right==3, mid==(left+right)/2==1, nums[target]<=nums[mid]时表示left==0,right==1, 否则left==2,right==3, 因此不管哪种情况最终只有两个元素需要考虑，退出时left==right，如果退出之前是nums[target]<=nums[mid], 则表示left、right都指向target==nums[mid], 否则指向nums[right], 如果在遍历过程中有过nums[target]==nums[mid]，那么right则一直指向target，因此最终退出时如果nums[right]!=target，则表示序列中不含target，否则left和right都会指向target。
接下来类似的方法寻找结束点。从起始点开始寻找。寻找思路是，当mid指向target时则移动rightStart到mid，这样可以保证rightStart一直指向target, 如果mid所指元素大于target，则缩小右边的rightEnd。最终也会只剩下两个元素需要考虑，当rightStart==rightEnd相等时均指向target。
- 实现
```
ass Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int size = nums.size();
        if(size == 0) return {-1, -1};
        int left = 0, right = size - 1, mid = -1;
        while(left < right)
        {
            mid = (left + right)/2;
            if(target <= nums[mid]) 
                right = mid;
            else left = mid + 1;
        }
        if(nums[left] !=  target) return {-1, -1};
        int rightStart = left, rightEnd = size - 1;
        while(rightStart < rightEnd)
        {
            int rightMid = (rightStart + rightEnd)/2 + 1;
            if(target == nums[rightMid]) rightStart = rightMid;
            else rightEnd = rightMid - 1;
        }
        return {left, rightStart};
    }
};
```
