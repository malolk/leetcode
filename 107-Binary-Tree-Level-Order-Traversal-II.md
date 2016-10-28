## NO.107. Binary Tree Level Order Traversal II 

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its bottom-up level order traversal as:
```
[
  [15,7],
  [9,20],
  [3]
]
```

## 思路
使用广度优先搜索，然后将最终所得的结果进行逆序输出。

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        if(root == NULL) return vector<vector<int>>();
        stack<TreeNode*> stk;
        vector<vector<int>> ret;
        stk.push(root);
        while(!stk.empty())
        {
            vector<TreeNode*> buf;
            while(!stk.empty())
            { 
                buf.push_back(stk.top());
                stk.pop();
            }
            vector<int> bufItem;
            for(int i = buf.size() - 1; i >= 0; --i)
            {
                bufItem.push_back(buf[i]->val);
                if(buf[i]->left) stk.push(buf[i]->left);
                if(buf[i]->right) stk.push(buf[i]->right);
            }
            ret.push_back(bufItem);
        }
        return vector<vector<int>>(ret.rbegin(), ret.rend());
    }
};
```