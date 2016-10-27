## NO.114. Flatten Binary Tree to Linked List
Given a binary tree, flatten it to a linked list in-place.

For example,
Given
```
         1
        / \
       2   5
      / \   \
     3   4   6
```
The flattened tree should look like:
```
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

## 思路
实现使用先序遍历，每次遍历时将子树的首尾返回，然后将左右子树进行连接。

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
        TreeNode* impl(TreeNode* root)
        {
            if(root == NULL) return NULL;
            TreeNode *leftEnd = NULL, *rightEnd = NULL;
            TreeNode *leftStart = root->left, *rightStart = root->right;
            root->left = NULL;
            rightEnd = impl(rightStart);
            leftEnd = impl(leftStart);
            if(leftStart)
            {
                root->right = leftStart;
                leftEnd->right = rightStart;
                if(rightStart) return rightEnd;
                else return leftEnd;
            }
            else 
            {   
                root->right = rightStart;
                if(rightStart) return rightEnd;
                else return root;
            }
        }
        void flatten(TreeNode* root) {
            impl(root);
        }
};

```