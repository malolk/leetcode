## NO.4 Median of Two Sorted Arrays

## 思路
两个数组A[M]和B[N], 设定两个分割索引i（0～M）和 j（0～N），i 和 j分别将两个数组分割为左右两部分，
```
A_left(A[0], A[i-1])、A_right(A[i], A[M-1])
B_left(B[0], B[j-1])、B_right(B[j], B[N-1])
```
只需要如下条件：
* A_left和B_left的长度为（M+N+1）/2，即i + j = (M+N+1)/2;
* A[i-1] <= B[j] && B[j-1] <= A[i]

这里需要注意一个问题，当M+N为奇数时，中位数为max（A[i-1], B[j-1]）,为偶数时，则为x_left的最大值与x_right的最小值的平均值。
另外还需要考虑i，j的边界问题，假如i\==0,则A没有左半部分，可知左半部分全在B中，若M+N为奇数，则中位值为B[j-1]， 偶数则为B[j-1]和min（A[i], B[j]）的平均值，此时搜索停止，j\==0的情况类似；假如i\==M，则A没有右边部分的值，右边部分的最小值则为B的右边部分的最小值，j\==N的情况类似。

## 实现代码

```
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
	int size1 = nums1.size(), size2 = nums2.size();
    if(!size1 && size2 ) return (size2 & 0x01) ? nums2[(size2-1)/2] : (nums2[(size2-1)/2] + nums2[(size2-1)/2 + 1])/2.0;
    else if(size1 && !size2) return (size1 & 0x01) ? nums1[(size1-1)/2] : (nums1[(size1-1)/2] + nums1[(size1-1)/2 + 1])/2.0;
    else if(!size1 && !size2) return 0;
    if(size1 > size2) return findMedianSortedArrays(nums2, nums1);
    int imin = 0, imax = size1, i = 0, j = 0;
    const int half_len = (size1 + size2 + 1)/2;
    while(imin <= imax)
    {
    	i = (imin + imax) >> 1;
        j = half_len - i;
        if(j > 0 && i < size1 && nums2[j - 1] > nums1[i])
        	imin = i + 1;
        else if(i > 0 && j < size2 && nums1[i - 1] > nums2[j])
            imax = i - 1;
        else
        {
            int max_of_left = 0, min_of_right = 0;
            if(i == 0) max_of_left = nums2[j - 1];
            else if(j == 0) max_of_left = nums1[i - 1];
            else max_of_left = max(nums1[i - 1], nums2[j - 1]);
            if((size1 + size2)&0x01)
            	return max_of_left;
            if(i == size1) min_of_right = nums2[j];
            else if(j == size2) min_of_right = nums1[i];
            else min_of_right = min(nums1[i], nums2[j]);
           	return (max_of_left + min_of_right)/2.0;
        }
        return 0;
    }
}
```