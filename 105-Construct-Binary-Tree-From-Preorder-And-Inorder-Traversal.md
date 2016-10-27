## NO.105. Construct Binary Tree from Preorder and Inorder Traversal
Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

## 思路
在每一个递归过程中，首先选取preorder的第一个元素为root，然后查找其在inorder中的位置index，则在index的左边为root的左子树，index的右边为root的右子树，同时可以确定左子树在preorder中的范围以及右子树在preorder中的范围。接着递归求解左右子树。

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
    TreeNode* impl(vector<int>& preorder, int ps, int pe,  
                    vector<int>& inorder, int is, int ie, unordered_map<int, int>& map)
    {
        if(ps == pe) return NULL;
        TreeNode* root = new TreeNode(preorder[ps]);
        int index = map[preorder[ps]];
        root->left = impl(preorder, ps + 1, index - is + ps + 1, inorder, is, index, map);
        root->right = impl(preorder, index - is + ps + 1, pe, inorder, index + 1, ie, map);
        return root;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        unordered_map<int, int> map;
        for(int i = 0; i < inorder.size(); ++i) map[inorder[i]] = i;
        return impl(preorder, 0, preorder.size(), inorder, 0, inorder.size(), map);
    }
};
```
