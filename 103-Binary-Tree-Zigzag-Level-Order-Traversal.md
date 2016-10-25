## NO.103. Binary Tree Zigzag Level Order Traversal

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7

```
return its zigzag level order traversal as:
```
[
  [3],
  [20,9],
  [15,7]
]
```

## 思路
使用异或操作符号控制遍历的顺序，使用stack来实现层级遍历。
注意stack会改变遍历的顺序，因此在改变遍历顺序时应该将其考虑在内。

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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ret;
        unsigned char flip = 1;
        stack<TreeNode*> buf;
        if(root) buf.push(root);
        while(!buf.empty())
        {
            vector<TreeNode*> bufNode;
            while(!buf.empty())
            {
                bufNode.push_back(buf.top());
                buf.pop();
            }
            vector<int> retTmp;
            if(!flip)
            {
                for(vector<TreeNode*>::iterator it = bufNode.begin(); it != bufNode.end(); ++it)
                    retTmp.push_back((*it)->val);
            }
            else
            {
                for(vector<TreeNode*>::reverse_iterator it = bufNode.rbegin(); it != bufNode.rend(); ++it)
                    retTmp.push_back((*it)->val);
            }
            ret.push_back(retTmp);
            flip ^= 0x01;
            for(vector<TreeNode*>::reverse_iterator it = bufNode.rbegin(); it != bufNode.rend(); ++it)
            {
                if((*it)->left) buf.push((*it)->left);
                if((*it)->right) buf.push((*it)->right);
            }
        }
        return ret;
    }
};
```

