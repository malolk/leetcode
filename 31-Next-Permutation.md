## Problem
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.
If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).
The replacement must be in-place, do not allocate extra memory.
Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
## 思路
既然是按照字母序比较大小，那么下一个置换应该尽量改变后面最短的数字串，从后面寻找子序列的下一个置换，如果没有则扩展子序列的长度直到找到可增的子序列串或者子序列串等于整个序列。因此从最后开始找，找到第一个逆序的位置，即`nums[i] > nums[i-1]`, 位置`i-1`则为逆序位置，位置`i-1`处与其后的任意一个比nums[i-1]大的位置置换后都比当前序列大，但是要求是下一个置换，则表明应该是大于nums[i-1]中最小的那个数，置换完毕后，位置`i-1`之后的子序列是一个最大序列，应该将其重新排序，由于该子序列是降序排列，因此只需要对称交换就可以。
## 问题
在实际实现时，注意一些边界条件，在寻找逆序元素时可能存在相同的元素，因此应该使用带有等号的不等式，在进行交换排序时，注意索引的边界值。
## 实现代码
```
class Solution {
	public:
		void nextPermutation(vector<int>& nums) {
			int size = nums.size();
			if(size < 2) return;
			int i = size - 1, k = -1;
			while(i > 0 && (nums[i] <= nums[i - 1]))
				--i;
			if(i)
			{
				k = i;
				while(k + 1 < size && nums[k + 1] > nums[i - 1])
					k++;
				swap(nums[k], nums[i - 1]);
			}
			for(int index = i; index < i + (size - i)/2; ++index)
				swap(nums[index], nums[size - 1 - (index - i)]);
		}
};
```
