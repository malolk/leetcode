## NO.95. Unique Binary Search Trees II
Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1...n.

For example,
Given n = 3, your program should return all 5 unique BST's shown below.
```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

## 思路
使用map存放已经构建的树，从而对递归进行优化。

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
    TreeNode* copyTree(TreeNode* root)
    {
        if(root == NULL) return root;
        TreeNode* newRoot = new struct TreeNode(root->val);
        newRoot->left = copyTree(root->left);
        newRoot->right = copyTree(root->right);
        return newRoot;
    }
    
    vector<TreeNode*> impl(int start, int end, unordered_map<string, vector<TreeNode*>>& map)
    {
        vector<TreeNode*> ret;
        string key = to_string(start) + "|" + to_string(end);
        if(map.count(key))
        {
            ret = map[key];
            for(vector<TreeNode*>::iterator it = ret.begin();
                it != ret.end(); ++it)
                (*it) = copyTree(*it);
            return ret;
        }
        if(start > end) return { NULL };
        for(int i = start; i <= end; i++)
        {
            vector<TreeNode*> leftPart = impl(start, i - 1, map);
            vector<TreeNode*> rightPart = impl(i + 1, end, map);
            int sum = leftPart.size() * rightPart.size();
            for(int l = 0; l < leftPart.size(); ++l)
                for(int r = 0; r < rightPart.size(); ++r)
                {
                    TreeNode* root = new struct TreeNode(i);
                    root->left = leftPart[l];
                    root->right = rightPart[r];
                    ret.push_back(root);
                }
        }
        map[key] = ret;
        return ret;
    }
    vector<TreeNode*> generateTrees(int n) {
        if(n == 0) return {};
        unordered_map<string, vector<TreeNode*>> map;
        return impl(1, n, map);
    }
};
```