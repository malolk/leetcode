## NO.99 Recover Binary Search Tree

Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.
Note:
A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?

## 思路
1. 使用递归进行中序遍历。记录前一个节点，初始时使用一个包含INT_MIN值的prev节点，然后使用当前节点与prev节点进行比较。此时有两种情况需要考虑，如果需要交换的节点是连续的或者是不连续的。
	- 不连续时，可以保证当prev > root时，如果需要交换的第一个节点没有被赋值，则可以确定prev是需要交换的节点，当再次遇到prev > root时则可以确定root为需要交换的节点。
	- 当连续时，prev > root只会出现一次，因此在第一次遇到时需要将需要交换的节点分别赋值为prev和root，即使第二次还会遇到则只需要改变第二个交换节点。

2. 使用Morris遍历的方式对树进行中序遍历。其他方式与上一个方法一样。

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
    void impl(TreeNode* root)
    {
        if(root == NULL) return;
        impl(root->left);
        if(!node1 && prev->val >= root->val)
            node1 = prev;
        if(node1 && prev->val >= root->val)
            node2 = root;
        prev = root;
        impl(root->right);
    }
    void recoverTree(TreeNode* root) {
        TreeNode node(INT_MIN);
        prev = &node;
        impl(root);
        if(!node1 || !node2) return;
        int tmp = node1->val;
        node1->val = node2->val;
        node2->val = tmp;
    }
private:
    TreeNode *node1 = NULL, *node2 = NULL;
    TreeNode *prev = NULL;
};

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
    void processNode(TreeNode* root)
    {
        if(prev && !node1 && prev->val > root->val)
        {
            node1 = prev; 
            node2 = root;
        }
        else if(prev && node1 && prev->val > root->val)
            node2 = root;
        prev = root;
    }
    void recoverTree(TreeNode* root) {
        while(root)
        {
            if(!root->left)
            {
                processNode(root);
                root = root->right;
            }
            else
            {
                TreeNode* predecessor = root->left;
                while(predecessor->right && predecessor->right != root)
                    predecessor = predecessor->right;
                if(predecessor->right == root)
                {
                    predecessor->right = NULL;
                    processNode(root);
                    root = root->right;
                }
                else
                {
                    predecessor->right = root;
                    root = root->left;
                }
            }
        }
        if(!node1 || !node2) return;
        int tmp = node1->val;
        node1->val = node2->val;
        node2->val = tmp;
    }
private:
    TreeNode *node1 = NULL, *node2 = NULL;
    TreeNode *prev = NULL;
};
```
