## NO.42 Trapping Rain Water

- Problem
 Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

 For example,
 Given [0,1,0,2,1,0,1,3,2,1,2,1], return 6. 
![rainwatertrap](./42-rainwatertrap.png)
The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped. 


- 思路
计算每一个位置上的水量需要知道左边最高高度和右边最高高度。采用双指针的方法，分别从左右两边开始。设指针分别为left和right。在每一个循环中开始之前必须计算left之前（包括left）的最大值maxleft和right之后（包括right）的最大值maxright。我们先讨论maxleft小于等于maxright的情况，此时可以确定不管left和right之间还会有什么高度值出现，left所值的水量仅由maxleft决定，此时right所指的水量是没法确定的，因为left和right之间可能有大于maxright的值出现，所以需要将left向右移，在右移的过程中，只要maxleft小于等于maxright，我们都可以确定当前left所指的水量，知道maxleft大于maxright，此时就可计算right所指的值。
值得注意的地方是，由于maxleft和maxright初始化为0, 因此需要在比较maxleft和maxright之前，需要计算[0, left]的maxleft和[right, size-1]的maxright。

- 实现
```
class Solution {
public:
    int trap(vector<int>& height) {
        int size = height.size();
        int sum = 0, maxLeft = 0, maxRight = 0;
        int left= 0, right = size - 1;
        while(left < right)
        {
            maxLeft = max(maxLeft, height[left]);
            maxRight = max(maxRight, height[right]);
            if(maxLeft <= maxRight)
            {
                if(height[left] < maxLeft) sum += maxLeft - height[left];
                ++left;
            }
            else
            {
                if(height[right] < maxRight) sum += maxRight - height[right];
                --right;
            }
        }
        return sum;
    }
};
```
