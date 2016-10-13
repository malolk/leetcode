## 33. Search in Rotated Sorted Array
- Problem
Suppose a sorted array is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
You are given a target value to search. If found in the array return its index, otherwise return -1.
You may assume no duplicate exists in the array.
- 思路
第一步先判断原先序列是上升还是下降，判断方法，首先比较第一个元素与最后一个元素的大小
* 如果是大，那么则元序列可能是降序也可能是经过旋转后的升序，因此还需判断中间元素与两端元素的顺序关系，如果三者关系是left > mid > right,则表示原序列是降序，否则为升序排列。
* 如果是小，那么则元序列可能是升序也可能是经过旋转后的降序，因此还需判断中间元素与两端元素的顺序关系，如果三者关系是left < mid < right,则表示原序列是升序，否则为降序排列。
第二步根据排列顺序分别进行二分查找，以升序为例
* 算出mid的位置，比较target与mid位置元素大小
* 如果小，则需判断target是在left和mid之间吗？或者left与mid之间有旋转点（nums[mid] < nums[left]）
* 如果大，则需判断target是在right和mid之间吗？或者right与mid之间有旋转点（nums[mid] > nums[right]）
- 注意边界问题
在实现中采用的是mid = (left + right + 1) /2的方式，mid<=right，另外在第二步中均使用带等号的不等式，因此需要预先判断mid==right的情况。
- 实现
```
class Solution {
	public:
		int search(vector<int>& nums, int target) {
			int size = nums.size();
			if(size == 0) return -1; 
			else if(size == 1) return nums[0] == target ? 0 : -1; 
			else if(size == 2) return nums[0] == target ? 0 : (nums[1] == target ? 1 : -1);
			bool isAscending = true;
			if(nums[0] > nums[size - 1] && nums[1] < nums[0] && nums[1] > nums[size - 1] ||
					nums[0] < nums[size - 1] && (nums[1] < nums[0] || nums[1] > nums[size - 1])) 
				isAscending = false; 
			int left = 0, right = size - 1, mid = -1; 
			while(left <= right)
			{
				mid = (left + right + 1)/2;
				if(mid == right) 
				{
					if(nums[left] == target) return left;
					else if(nums[right] == target) return right;
					else return -1; 
				}
				if(target == nums[mid]) return mid;
				if(isAscending)
				{
					if(target < nums[mid])
					{
						if(nums[mid] <= nums[left] || target >= nums[left] ) right = mid - 1;
						else if(target <= nums[right]) left = mid + 1;
						else return -1; 
					}
					else
					{   
						if(nums[mid] >= nums[right] || target <= nums[right]) left = mid + 1;
						else if(target >= nums[left]) right = mid - 1;
						else return -1; 
					}   
				}
				else
				{
					if(target < nums[mid])
					{
						if(nums[mid] <= nums[right] || target >= nums[right]) left = mid + 1;
						else if(target <= nums[left]) right = mid - 1;
						else return -1; 
					}
					else
					{
						if(nums[mid] >= nums[left] || target <= nums[left]) right = mid - 1;
						else if(target >= nums[right]) left = mid + 1;
						else return -1; 
					}
				}
			}   
			return -1; 
		}
};
```
