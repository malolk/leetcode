## NO.16 3Sum Closest

Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

>  For example, given array S = {-1 2 1 -4}, and target = 1.
>  The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

## 思路

首先对数组元素进行排序，在每个循环中，首先固定第一个元素，然后在余下的元素中找剩下的两个元素。寻找的规则：分别从开头和结尾两边进行遍历， 索引为i， j，如果所得的三个数的和小于target，则i\+\+， 小于则\-\-j，等于则直接返回所得和，循环结束的条件是i>=j, 其中遍历时记住与target的绝对差最小的数。

	* 总结： 对于两个变量动态调整时达到最优化的值时，可以对范围内的元素进行两边同时索引，根据与目标的比较对应的调整两边的索引，从而整个遍历过程为O（N）。

## 实现代码

```
int threeSumClosest(vector<int>& nums, int target) {
	int diff = INT_MAX; 
	sort(nums.begin(), nums.end());
	int ret = nums[0] + nums[1] + nums[2];
	if(ret == target) return ret;
	for(int index = 0; index < nums.size() - 2; ++index)
	{
		if(index > 0 && nums[index - 1] == nums[index]) continue;
		int i = index + 1, j = nums.size() - 1;
		while(i < j)
		{
			int sum = nums[index] + nums[i] + nums[j];
			if(abs(sum - target) < abs(ret - target))
				ret = sum;
			if(sum < target) ++i;
			else if(sum > target) --j;
			else return sum;
		}
	}
	return ret;
}
```
