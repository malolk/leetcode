## NO.116. Populating Next Right Pointers in Each Node
Given a binary tree

    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

    You may only use constant extra space.
    You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

For example,
Given the following perfect binary tree,

         1
       /  \
      2    3
     / \  / \
    4  5  6  7

After calling your function, the tree should look like:

         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL

## 思路
每次将相邻的节点进行递归，将相邻节点进行连接，接着使用递归连接子节点。每次有三对相邻的节点。
优化：在递归之前判断当前的相邻节点是否已经连接，如果已经连接则说明其子树中的节点也已经连接。

## 实现
```
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void impl(TreeLinkNode* left, TreeLinkNode* right)
    {
        if(!left || !right) return;
        if(left->next == right) return;
        left->next = right;
        impl(left->left, left->right);
        impl(left->right, right->left);
        impl(right->left, right->right);
    }
    void connect(TreeLinkNode *root) {
        if(root == NULL) return;
        return impl(root->left, root->right);
    }
};
```