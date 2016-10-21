## NO.84. Largest Rectangle in Histogram
Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram. 

![](./84-histogram.png)
Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].
![](./84-histogram_area.png)
The largest rectangle is shown in the shaded area, which has area = 10 unit.

For example,
Given heights = [2,1,5,6,2,3],
return 10. 

## 思路
1. 使用分治算法
每次选取一个范围，使用[segment-tree](http://www.geeksforgeeks.org/segment-tree-set-1-range-minimum-query/)获得这个范围中的最小数的index，那么此范围中的最大值就是
	- 使用index处的值乘以范围的长度
	- index左边范围的最大值(不包括index)
	- index右边范围的最大值（不包括index）
这三项中最大值。其中i求取index左右两边的最大值可以使用递归。其中的难点是怎么高效的查找某个给定范围内的最小值的索引，此解法中使用了segment-tree，注意此数据结构中的节点的个数最大值为`2 * pow(2, ceil(log2(size))) - 1`，就是和具有相同层数的完全树的节点数相同。

2. 使用stack来存储index
可以将求取最大值分解为以下步骤，首先获取以每一个位置上的高度heights[i]作为最小值而形成的最大矩形面积，然后求取这些面积中的最大值极为要求的矩形最大面积。
**如何确定并获得以i处的高度作为最低值形成的矩形面积？**
只要获得在索引i的左边第一个比heights[i]小的索引left， 以及在索引i的右边第一个比heights[i]小的索引值right。则在索引范围(left, right)之间便是形成矩形区域。
如何实施这样一个思路？首先从左到右试图将每个位置的索引压入栈中，只有当栈为空或者当前索引位置的高度大于等于栈顶存储的索引位置的高度才将其压入栈中。否则，此时就可确定以栈顶存储的索引位置的高度为最小高度的右边界限，即上述中的right，即为当前试图压入的索引值，left如何确定？这时需要考虑高度相同的索引值相继存在于栈中，因此需要不断pop掉与当前栈顶索引高度相同的元素直到栈顶存储的索引值所指示的高度小于初始栈顶索引值所指示的高度值或者栈为空，这时便可以确定以初始栈顶存储的索引值所指示的高度为矩形高的矩形范围，如果栈为空则表示当前试图压入的索引即为矩形的宽度。
最后可能会出现栈中还留存元素，以升序排列，计算方法与上述相同。或者在初始的输入数组最后加一个0则可以保证循环结束后栈中不会有初始输入数组中的索引。
## 实现
```
class Solution {
public:
    int buildMinSegmentTree(vector<int>& inputs, int rs, int re,
                                vector<int>& st, int nodeIndex)
    {
        if(rs == re)    st[nodeIndex] = rs;
        else
        {
            int mid = (rs + re)/2 ;
            int leftIndex = buildMinSegmentTree(inputs, rs, mid, st, 2*nodeIndex + 1);
            int rightIndex = buildMinSegmentTree(inputs, mid + 1, re, st, 2*nodeIndex + 2);
            st[nodeIndex] = (inputs[leftIndex] < inputs[rightIndex]) ? leftIndex : rightIndex;
        }
        return st[nodeIndex];
    }
    int getRangeMinIndex(vector<int>& inputs, int rs, int re,
                            vector<int>& st, int nodeIndex,
                            int qs, int qe)
    {
        if(qs <= rs && qe >= re) return st[nodeIndex];
        if(qe < rs || qs > re) return -1;
        int mid = (rs + re)/2;
        int leftIndex = getRangeMinIndex(inputs, rs, mid, st, nodeIndex*2 + 1, qs, qe);
        int rightIndex = getRangeMinIndex(inputs, mid + 1, re, st, nodeIndex*2 + 2, qs, qe);
        if(leftIndex == -1) return rightIndex;
        else if(rightIndex == -1) return leftIndex;
        else return (inputs[leftIndex] < inputs[rightIndex]) ? leftIndex : rightIndex;
    }
    
    int largestRectangleAreaImpl(vector<int>& heights, vector<int>& st, int start, int end)
    {
        if(start == end) return heights[start];
        else if(start > end) return 0;
        int minIndex = getRangeMinIndex(heights, 0, heights.size() - 1, st, 0, start, end);
        int curMax = heights[minIndex]*(end - start + 1);
        int leftMax = largestRectangleAreaImpl(heights, st, start, minIndex - 1);
        int rightMax = largestRectangleAreaImpl(heights, st, minIndex + 1, end);
        return max(curMax, max(leftMax, rightMax));
    }
    int largestRectangleArea(vector<int>& heights) {
        int size = heights.size();
        if(size == 0) return 0;
        vector<int> st(2 * pow(2, ceil(log2(size))) - 1, 0);
        buildMinSegmentTree(heights, 0, size - 1, st, 0);
        return largestRectangleAreaImpl(heights, st, 0, size - 1);
    }
};
```
```
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        heights.push_back(0);
        int size = heights.size();
        stack<int> indexes;
        int maxArea = 0;
        for(int i = 0; i < size;)
        {
            int cur = heights[i];
            if(indexes.empty() || cur >= heights[indexes.top()])
                indexes.push(i++);
            else
            {
                int tp = indexes.top();
                int h = heights[tp];
                indexes.pop();
                while(!indexes.empty() && heights[indexes.top()] == h)
                {
                    tp = indexes.top();
                    indexes.pop();
                }
                int area = (indexes.empty() ? i : (i - indexes.top() - 1)) * h;
                maxArea = max(maxArea, area);
            }
        }
        return maxArea;
    }
};
```