## NO.112. Path Sum
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.
For example:
Given the below binary tree and sum = 22,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

## 思路
使用深度优先搜索，每往下遍历一次将sum减去节点中的值，判断当前节点是否是叶子节点且sum==0, 如果是则二叉树中存在一条路径其节点的和为sum。

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
    bool hasPathSum(TreeNode* root, int sum) {
        if(root == NULL) return false;
        if((sum -= root->val) == 0)
            if(!root->left && !root->right) 
                return true;
        if(hasPathSum(root->left, sum) || 
            hasPathSum(root->right, sum))
            return true;
        else return false;
    }
};
```