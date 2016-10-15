NO.41 First Missing Positive

- Problem
 Given an unsorted integer array, find the first missing positive integer.
 For example,
 Given [1,2,0] return 3,
 and [3,4,-1,1] return 2.
 Your algorithm should run in O(n) time and uses constant space. 

- 思路
输入的size为N，首先将(0, N]之内的数存放在正确的位置，比如nums[i] == 5, 则应该与nums[4]交换。在实现中使用while循环直到i处的值为i + 1, 或者nums[i]处的值为负数，或者其值超过了N, **为什么一定要使用while？**, 因为存在这样的情况，比如nums[0]==5, 交换后，nums[0]==6, 而如果nums[1]经过交换后变为1, 则结果将会变为1，显然不对，也就是说使用if并不能使得数值出现在正确的位置。交换完毕后，可以保证原来输入中的元素在(0, N]范围内的值一定也会出现在nums的[0, K-1]之内，其中K为小于等于N的最大值。 现在分情况讨论。
	1. 假如位置[0, K-1]之内存在负数或0, 则第一次出现负数或0时的位置加上1则为结果；
	2. 假如位置[0, K-1]之内全为正数则一定连续，如果不是从1开始则结果为1；
	3. 如果是从1开始，此时讨论K的大小，如果K小于N，[K, N]之内的正数又都大于N，因此结果为K + 1；
	4. 如果K等于N，则表示输入的数全部为连续正数且起始为1, 则结果为N + 1。
第1、2和3中情况只需检查交换后的nums中，第i处的值是否等于i+1, i从0开始。第4种情况说明当前nums中是有序连续且从1开始的nums，则只需返回N+1。

- 实现
```
ass Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int size = nums.size();
        for(int i = 0; i < size; ++i)
        {
            while(nums[i] <= size && nums[i] > 0 && nums[i] != nums[nums[i] - 1])
                swap(nums[i], nums[nums[i] - 1]);
        }
        for(int i = 0; i < size; ++i)
        {
            if(nums[i] != i + 1) return i + 1;
        }
        return size + 1;
    }
};
```
