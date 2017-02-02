## NO.321 Create Maximum Number

## Problem
Given two arrays of length m and n with digits 0-9 representing two numbers. Create the maximum number of length k <= m + n from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the k digits. You should try to optimize your time and space complexity.

Example 1:

nums1 = [3, 4, 6, 5]
nums2 = [9, 1, 2, 5, 8, 3]
k = 5
return [9, 8, 6, 5, 3]

Example 2:

nums1 = [6, 7]
nums2 = [6, 0, 4]
k = 5
return [6, 7, 6, 0, 4]

Example 3:

nums1 = [3, 9]
nums2 = [8, 9]
k = 3
return [9, 8, 9] 

## 思路
基本的思路分为以下两个步骤：
首先分别针对第一个数组求取n个数字组成的最大数s1，对第二个数组求取m个数字组成的最大数s2，其中m+n=k
然后，将两个数字s1和s2进行归并操作得到s3
每次调整不同的m和n，进行以上操作并记录最大的值。

值得注意的点：
1. 在求取一个数组由n个数字组成的最大数s时，使用栈的思想，只有在以下条件满足时才会将栈顶元素弹出，１）栈顶元素小于当前要压入的元素；２）栈中的元素加上未遍历的元素的个数大于n；３）栈不为空。在将可以弹出的元素弹出后，需要判断当前的栈是否已经满了（因为可能此时栈已满，但是当前的元素小于或者等于栈顶元素），如果栈中有空闲位置，则将当前的值压入到栈中。

2. 在进行归并时需要注意的是，如果两个数组当前的值是相等的，则需要比较后续的值是否相等，应该选择后续元素较大的那个。其中有一种情况，如果其中一个数组是另一个数组的子串，则较长的数组应该判为较大。

## 实现
```
class Solution {
public:
    vector<int> maxArray(vector<int>& nums, int k) {
        if (k == 0) {
            return {};
        }
        vector<int> ret(k);
        int size = nums.size();
        for (int i = 0, r = 0; i < size; ++i) {
            while (r > 0 && (size - i + r) > k && nums[i] > ret[r - 1]) {
                r--;
            }
            if (r < k) {
                ret[r++] = nums[i];
            }
        }
        
        return ret;
    }
    
    bool greater(const vector<int>& nums1, int i, const vector<int>& nums2, int j) {
        int n1 = nums1.size(), n2 = nums2.size();
        while (i < n1 && j < n2 && nums1[i] == nums2[j]) {
            ++i;
            ++j;
        }
        
        return j == n2 || i < n1 && nums1[i] > nums2[j];
    }
    
    vector<int> merge(const vector<int>& nums1, const vector<int>& nums2) {
        int n1 = nums1.size(), n2 = nums2.size();
        int size = n1 + n2;
        vector<int> ret(size);
        int i = 0, j = 0, index = 0;
        while (index < size) {
            if (j == n2 || i < n1 && greater(nums1, i, nums2, j)) {
                ret[index++] = nums1[i++];
            } else {
                ret[index++] = nums2[j++];
            }
        }
        return ret;
    }
    
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {
        vector<int> ret(k, 0);
        int n1 = nums1.size(), n2 = nums2.size();
        for (int i = max(0, k - n2); i <= n1 && i <= k; ++i) {
            int left = k - i;
            vector<int> ans = merge(maxArray(nums1, i), maxArray(nums2, left));
            if (greater(ans, 0, ret, 0)) {
                ret.swap(ans);
            } 
        }
        return ret;
    }
};
```
