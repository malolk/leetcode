## NO.94. Binary Tree Inorder Traversal
Given a binary tree, return the inorder traversal of its nodes' values.

For example:
Given binary tree [1,null,2,3], return [1,3,2]. 

## 思路
每一个循环首先将根节点以及所有左子节点压入栈中，然后再将栈顶的节点（即最左节点）的右节点设置为根节点，进入下一个循环。当没有节点压入栈中时表示所有节点已经遍历完毕。

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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ret;
        stack<TreeNode*> stk;
        while(true)
        {
            while(root)
            {
                stk.push(root);
                root = root->left;
            }
            if(stk.empty()) break;
            root = stk.top();
            stk.pop();
            ret.push_back(root->val);
            root = root->right;
        }
        return ret;
    }
};
```