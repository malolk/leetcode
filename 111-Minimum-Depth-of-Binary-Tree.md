## NO.111. Minimum Depth of Binary Tree
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

## 思路
使用广度优先搜索，当每一层的节点中存在叶子节点时返回层数。
使用深度优先搜索，对于每一个节点分别判别左右子节点是否为空，从而实行不同的策略。

## 实现
```
depth-first search
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
    int impl(TreeNode* root)
    {
        if(root == NULL)
            return 0;
        if(!root->left && !root->right) return 1;
        else if(root->left && !root->right) return 1 + impl(root->left);
        else if(!root->left && root->right) return 1 + impl(root->right);
        else 
        {
            int left = impl(root->left);
            int right = impl(root->right);
            return min(left, right) + 1;
        }
    }
    int minDepth(TreeNode* root) {
        return impl(root);
    }
};
```

```
breadth-first search
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
    int minDepth(TreeNode* root) {
        stack<TreeNode*> stk;
        int depth = 0;
        if(root)    stk.push(root);
        while(!stk.empty())
        {
            ++depth;
            vector<TreeNode*> buf;
            while(!stk.empty())
            {
                buf.push_back(stk.top());
                stk.pop();
            }
            for(auto item : buf)
            {
                if(!item->left && !item->right) return depth;
                if(item->left) stk.push(item->left);
                if(item->right) stk.push(item->right);
            }
        }
        return depth;
    }
};
```