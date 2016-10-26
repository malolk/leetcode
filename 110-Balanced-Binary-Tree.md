## NO.110. Balanced Binary Tree
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

## 思路
使用递归求解各层的高度，并顺便判断当前节点是否是平衡的。

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
    bool impl(TreeNode* root, int* depth)
    {
        if(root == NULL)
        {
            *depth = 0;
            return true;
        }
        int leftH = 0, rightH = 0;
        if(!impl(root->left, &leftH)) return false;
        if(!impl(root->right, &rightH)) return false;
        if((leftH - rightH) > 1 || (leftH - rightH) < -1) return false;
        *depth =  max(leftH, rightH) + 1;
        return true;
    }
    
    bool isBalanced(TreeNode* root) {
        int depth = 0;
        return impl(root, &depth);
    }
};
```