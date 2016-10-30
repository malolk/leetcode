## NO.129. Sum Root to Leaf Numbers

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,
```
    1
   / \
  2   3
```
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25. 

## 思路
使用深度优先搜索的思想，每次访问一个非叶子节点时将其附在num最后一位，每次递归返回时依次去除最后一位。每次访问到叶子节点时则将num加到结果中去。

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
    void impl(TreeNode* root, int& num, int& sum)
    {
        if(root == NULL) return;
        num = num*10 + root->val;
        if(!root->left && !root->right)
        {
            sum += num;
            num /= 10;
            return;
        }
        if(root->left) impl(root->left, num, sum);
        if(root->right) impl(root->right, num, sum);
        num /= 10;
    }
    int sumNumbers(TreeNode* root) {
        int ret = 0;
        int num = 0;
        impl(root, num, ret);
        return ret;
    }
};
```