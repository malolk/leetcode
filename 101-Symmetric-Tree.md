## NO.101. Symmetric Tree
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

    1
   / \
  2   2
 / \ / \
3  4 4  3

But the following [1,2,2,null,3,null,3] is not:

    1
   / \
  2   2
   \   \
   3    3

Note:
Bonus points if you could solve it both recursively and iteratively.

## 思路
递归：每次选择两个对称的节点leftN和rightN进行比较，接着使用leftN的左节点与rightN的右节点进行递归比较，以及将了leftN的右节点和rightN的左节点进行递归比较，只有这两项都为true才表示这棵树是对称的。

迭代： 使用stack实现广度优先搜索。

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
    bool impl(TreeNode* left, TreeNode* right)
    {
        if(left == NULL ^ right == NULL) return false;
        else if(left == NULL) return true;
        else if(left->val != right->val) return false;
        return impl(left->left, right->right) && 
                impl(left->right, right->left);
    }
    bool isSymmetric(TreeNode* root) {
        if(root == NULL) return true;
        return impl(root->left, root->right);
    }
};
```
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
    bool isSymmetric(TreeNode* root) {
        if(root == NULL) return true;
        stack<TreeNode*> stk;
        stk.push(root);
        while(!stk.empty())
        {
            if(stk.top() == root)
            {
                stk.pop();
                stk.push(root->left);
                stk.push(root->right);
                continue;
            }
            vector<TreeNode*> buf;
            while(!stk.empty())
            {
                buf.push_back(stk.top());
                stk.pop();
            }
            int size = buf.size();
            for(int i = 0; i < size; ++i)
            {
                if(i < size / 2)
                {
                    if(buf[i] == NULL ^ buf[size - 1 - i] == NULL || 
                        buf[i] && buf[i]->val != buf[size - 1 - i]->val) 
                            return false;
                }
                if(buf[i])
                {
                    stk.push(buf[i]->left);
                    stk.push(buf[i]->right);
                }
            }
        }
        return true;
    }
};
```
