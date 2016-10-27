## NO.106. Construct Binary Tree from Inorder and Postorder Traversal
Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

## 思路
在每一次递归中，使用postorder中的最后一个元素作为根节点，在inorder中查找根节点的位置，则index的左边即为根节点的左子树范围，右边即为根节点的右子树范围，即可确定postorder中左右子树的范围。接着使用递归求解左右子树。

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
    TreeNode* impl(vector<int>& postorder, int ps, int pe,
                    vector<int>& inorder, int is, int ie, unordered_map<int, int>& map)
    {
        if(ps == pe) return NULL;
        TreeNode* root = new TreeNode(postorder[pe - 1]);
        int index = map[postorder[pe - 1]];
        root->right = impl(postorder, pe - (ie - index), pe - 1, inorder, index + 1, ie, map);
        root->left = impl(postorder, ps, pe - (ie - index), inorder, is, index, map);
        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        unordered_map<int, int> map;
        for(int i = 0; i < inorder.size(); ++i) map[inorder[i]] = i;
        return impl(postorder, 0, postorder.size(), inorder, 0, inorder.size(), map);
    }
};
```