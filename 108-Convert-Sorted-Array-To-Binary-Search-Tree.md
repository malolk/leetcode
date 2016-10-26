## NO.108. Convert Sorted Array to Binary Search Tree

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

## 思路
使用二分法每次将最中间的元素设置为子树的根节点，依次递归求取各个子树的左右子树。

## 实现
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution
{
public:
    TreeNode* impl(vector<int>& nums, int start, int end)
    {   
        if(start == end)
            return new TreeNode(nums[start]);
        else if(start > end)
            return NULL;    
        int root = (start + end)/2;
        TreeNode* rootPtr = new TreeNode(nums[root]);
        rootPtr->left = impl(nums, start, root - 1); 
        rootPtr->right = impl(nums, root + 1, end);
        return rootPtr;
    }

    TreeNode* sortedArrayToBST(vector<int>& nums)
    {   
        return impl(nums, 0, nums.size() - 1);      
    }   
};
```
