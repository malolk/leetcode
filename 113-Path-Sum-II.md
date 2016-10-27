## NO.113. Path Sum II 
Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.
For example:
Given the below binary tree and sum = 22,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
return
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

## 思路
使用深度优先搜索的方法，每次将sum减去当前节点的值并判断sum是否为零且当前节点是否为叶子节点，如果是则将当前前缀保存到返回结果中，否则对左右字节点进行递归。在每次递归结束后都需要将之前存入前缀的节点删除。

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
class Solution {
public:
    void impl(TreeNode* root, int sum, vector<int>& prev, vector<vector<int>>& ret)
    {
        if(root == NULL) return;
        prev.push_back(root->val);
        if((sum -= root->val) == 0 &&
            !root->left && !root->right)
                ret.push_back(prev);
        else
        {
            impl(root->left, sum, prev, ret);
            impl(root->right, sum, prev, ret);
        }
        prev.pop_back();  /*it's for efficiency*/
    }
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<vector<int>> ret;
        vector<int> prev;
        impl(root, sum, prev, ret);
        return ret;
    }
};
```