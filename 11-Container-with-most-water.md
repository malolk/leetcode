## Problem

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water. 

## 思路

使用两个指针分别指向容器的左右两边，首先算最宽的容器的容量和高度h，然后如果两个指针中的任何一个小于等于h则向中间靠拢，因为宽度变小，所以必须新的高度比原先的高才有可能容量比原来的大，一直调整直到两边的高度都大于上次计算的高度才进行下一次计算，求得新的最大容量值。

## 实现代码

```
int maxArea(vector<int>& height) {
	int size = height.size();
	if(size <= 1) return 0;
	int water = 0;
	int index_i = 0, index_j = size - 1;
	int h = 0;
	while(index_i < index_j)
	{
		h = min(height[index_i], height[index_j]);
		water = max(water, (index_j - index_i)*h);
		while(height[index_i] <= h && index_i < (size - 1)) ++index_i;
		while(height[index_j] <= h && index_j > 0) --index_j;
	}
	return water;
}
```
